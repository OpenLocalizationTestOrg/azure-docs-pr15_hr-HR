<properties
    pageTitle="Kako funkcionira oporavak web-mjesta? | Microsoft Azure"
    description="Ovaj članak sadrži pregled arhitektura oporavak web-mjesta"
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
    ms.topic="get-started-article"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="how-does-azure-site-recovery-work"></a>Kako funkcionira oporavak Azure web-mjesta?

Ovaj članak da biste shvatili podlozi arhitektura servisa Azure oporavak web-mjesta i komponente koje ga čine raditi. 

Pri dnu ovog članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)objavljuju komentare ni pitanja.


## <a name="overview"></a>Pregled

Tvrtke i ustanove moraju strategije BCDR koja određuje kako aplikacija, radnih opterećenja i podataka ostati pokrenut i dostupne tijekom planirane i Neplanirana isključiti, a čim oporaviti normalno funkcionira uvjeta. Strategije BCDR trebali biste zadržali poslovnih podataka sigurnih i koje se mogu vratiti i provjerite je li radnih opterećenja ostaju stalno dostupne kada se pojavi Izrada. 

Oporavak web-mjesta je Azure service doprinosa strategije BCDR po orchestrating replikacije lokalne poslužitelje fizičke i virtualnih računala s oblakom (Azure) ili sekundarni podatkovnog centra. Prilikom kvarove u svom primarnom mjestu, koji se neće putem sekundarne mjesto da bi aplikacija i radnih opterećenja dostupna. Ne natrag na svom primarnom mjestu kada on se vraća na običnom operacije. Dodatne informacije u [oporavak web-mjesta?](site-recovery-overview.md)

## <a name="site-recovery-in-the-azure-portal"></a>Oporavak web-mjesta na portalu za Azure

Azure sadrži dvije različite [implementacije modela](../resource-manager-deployment-model.md) za stvaranje i rad s resursima: model upravljanja resursima Azure i model upravljanja klasičnom usluga. Azure ima dvije portalima – [Azure klasični portal](https://manage.windowsazure.com/) podržava model klasični implementacije i [Azure portal](https://portal.azure.com) s podrškom za oba modele implementacije.

Oporavak web-mjesta dostupan je u klasični portal i Azure portal. Na portalu za Azure klasični podržavaju oporavak web-mjesta s modelom upravljanja klasičnom usluga. Na portalu za Azure podržavaju klasični modela ili resursa Model implementacije. [Dodatne informacije potražite u](site-recovery-overview.md#site-recovery-in-the-azure-portal) o implementaciji pomoću portala za Azure.

Informacije u ovom članku odnose se na klasične i Azure portala implementacije. Razlike su zabilježene gdje je to primjenjivo.

## <a name="deployment-scenarios"></a>Scenariji za implementaciju

Oporavak web-mjesta mogu biti implementirano u orkestrirali replikacije broj scenarije:

- **Replicirati VMware virtualnim strojevima**: mogli ponoviti lokalnog VMware virtualnim strojevima Azure ili sekundarni podatkovnog centra.
- - **Replicirati fizičke strojeva**: mogli ponoviti fizičke strojeva izvodi Windows ili Linux Azure ili sekundarni podatkovnog centra. Postupak za replikaciju fizičke strojeva je gotovo jednako postupka za replikaciju VMware VMs
- **Replicirati VMs Hyper-V (bez VMM)**: mogli ponoviti Hyper-V VMs koji ne upravlja VMM za Azure.
- **Replicirati Hyper-V VMs upravlja se iz oblaka VMM centar sustava**: mogli ponoviti lokalnog Hyper-V virtualnim strojevima izvode na poslužiteljima glavno računalo Hyper-V u VMM oblaka Azure ili sekundarni podatkovnog centra. Možete replicirati pomoću standardnih Hyper-V replike ili pomoću SAN replikacije.
- **Migriranje VMs**: koristite oporavak web-mjesta će se [migrirati Azure IaaS VMs](site-recovery-migrate-azure-to-azure.md) među područjima ili će se [migrirati instance AWS Windows](site-recovery-migrate-aws-to-azure.md) da biste Azure IaaS VMs. Trenutno samo migracije podržano je što znači uspijeva iznad te VMs, ali ih nije moguće vratiti uspjeti.

Oporavak web-mjesta mogu replicirati Većina aplikacije koje se izvode na te VMs i fizičke poslužitelja. Potpuno možete dobiti sažetak podržanih aplikacija u [koje radnih opterećenja oporavak web-mjesta Azure štiti?](site-recovery-workload.md)


## <a name="replicate-to-azure-vmware-virtual-machines-or-physical-windowslinux-servers"></a>Repliciranje za Azure: VMware virtualnim strojevima ili fizičke Windows/Linux poslužitelja

Postoje dva načina za replikaciju VMware VMs s oporavak web-mjesta.

- **Pomoću portala za Azure**– kada implementacija oporavak web-mjesta na portalu Azure možete uspjeti putem VMs za pohranu Upravitelj klasičnom usluga ili upravitelju resursa. Na portalu za Azure replikaciju VMware VMs poziva brojne prednosti, uključujući mogućnost za replikaciju classic ili Voditelj resursa za pohranu u Azure. [Dodatne informacije](site-recovery-vmware-to-azure.md).
- **Pomoću portala za klasični**– možete implementirati oporavak web-mjesta na portalu klasični koristili Poboljšano sučelje. Time se koristiti za sve nove implementacijama klasični portalu. U ovom implementacije koje možete samo uspjeti putem VMs klasični pohranom na servisu Azure, a ne i Voditelj resursa za pohranu. [Dodatne informacije](site-recovery-vmware-to-azure-classic.md). Postoji [naslijeđene sučelje](site-recovery-vmware-to-azure-classic-legacy.md) za postavljanje VMware replikacije klasični portalu. To bi trebalo koristiti za nove implementacije.  Ako ste već implementiran pomoću na naslijeđene sučelje [Informirajte se o migraciji](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) poboljšane implementacije.



Arhitektonski preduvjeti za implementaciju oporavak web-mjesta za replikaciju VMs fizičke VMware poslužitelja web-mjesto portala za Azure ili u okvir za Azure klasični portal (Napredni) su slične nekoliko razlike:

- Ako morate je implementirati na portalu Azure možete replicirati utemeljen na Voditelj resursa za pohranu i koristiti resursima mreža o povezivanju sa servisom Azure VMs nakon prebacivanje.
- Kada implementacije na portalu za Azure i LRS i GRS prostora za pohranu podržana. Na portalu klasični potreban je GRS.
- Postupak implementacije je pojednostavnjeni i više jednostavnih na portalu za Azure.


Evo što vam je potrebno:

- **Račun za Azure**: potreban vam je račun za Microsoft Azure.
- **Azure prostora za pohranu**: morat ćete račun Azure prostora za pohranu za pohranu repliciranu podataka. Možete koristiti klasične računa ili računa za pohranu Voditelj resursa. Račun može biti LRS ili GRS ako pokrenete na portalu za Azure. Repliciranu podaci se pohranjuju u Azure prostora za pohranu i Azure VMs su spun se kada se pojavi prebacivanje. 
- **Azure mreže**: morat ćete Azure virtualne mreže koji Azure VMs će se povezati s kada su ste stvorili na prebacivanje. Na portalu za Azure zna biti mreže stvorene u modelu Upravitelj klasičnom usluga ili u modelu Voditelj resursa.
- **Lokalni poslužitelj za konfiguraciju**: morat ćete računalu Windows Server 2012 R2 za lokalni koji se izvodi poslužitelj za konfiguraciju i druge komponente oporavak web-mjesta. Ako ste replikaciju VMware VMs to mora biti vrlo dostupna VM VMware. Ako želite replicirati fizičke poslužitelje na računalu mogu biti fizičke. Te oporavak web-mjesta komponente instalirat će se na računalu:
    - **Poslužitelj za konfiguraciju**: koordinate komunikaciju između lokalnog okruženja i Azure i upravlja replikacije podataka i oporavak.
    - **Poslužitelj za postupak**: služi kao replikacije pristupnika. Ga prima replikacije podatke iz zaštićenog izvor strojeva, optimizira predmemoriranja, sažimanja i šifriranje i šalje podatke Azure prostora za pohranu. Također rukuje automatske instalacije servisa mobilnost na zaštićenom računalima, i izvodi automatsko otkrivanje VMware VMs. Kao što je rastom implementaciju sustava možete dodati dodatne drugi proces namjenski poslužitelji za rukovanje rastuće količine promet replikacije.
    - **Matrica poslužitelju ciljni**: rukuje replikacije podataka tijekom failback iz Azure. 
- **VMware VMs ili fizičke poslužitelji za replikaciju**: svakom računalu koje želite replicirati Azure ćete komponenta za servis mobilnost instaliran. Taj servis spremit će zapisivanje podataka na računalu i prosljeđuje ih na poslužitelj za postupak. Komponente možete ručno, instalirati ili možete se pomiču i automatski instalirao poslužitelj za postupak Kada omogućite replikacije za računalo.
- **vSPhere domaćini/vCenter poslužitelja**:, potreban vam je jedan ili više vSphere glavno računalo poslužitelje s programom VMware VMs. Preporučujemo da implementacije vCenter poslužitelj za upravljanje tim domaćini.
- **Failback**: Evo što vam je potrebno:
    - **Nije podržan fizičke fizičke failback**: to znači da ako neće uspjeti putem fizičke poslužitelji za Azure, a želite ponovno uspjeti, morate ne vratite VMware VM. Ne može se neće fizičke poslužitelj. Morat ćete programa Azure VM uvoza natrag, a ako niste implementacija poslužitelj za konfiguraciju kao VMware VM morat ćete postaviti zasebne osnovne cilj poslužitelju kao VMware VM. To je potrebno jer glavnog poslužitelja stupi u interakciju i pridružuje VMware prostora za pohranu da biste vratili na diskova VMware VM.
    - - **Poslužitelj privremeno postupak u Azure**: želite li uspjeti natrag od Azure nakon prebacivanje morat ćete postaviti programa Azure VM konfigurirati kao poslužitelj za postupak za rukovanje replikacije iz Azure. U ovom VM možete izbrisati završetku failback.
    - **VPN vezu**: failback morat ćete je VPN veza (ili Azure ExpressRoute) postavite Azure mrežu na lokalno mjesto.
    - **Zasebnom lokalnog osnovnih poslužitelju ciljni**: failback postupanja s lokalnog poslužitelja osnovne cilj. Glavni odredišni poslužitelj je instaliran po zadanom na poslužitelju za upravljanje, ali ako se ne uspijeva natrag veće količine promet na zasebnom lokalnog osnovne cilj poslužitelja trebali postaviti u tu svrhu.

**Općenito arhitekture**

![Poboljšana](./media/site-recovery-components/arch-enhanced.png)

**Implementacija komponente**

![Poboljšana](./media/site-recovery-components/arch-enhanced2.png)

**Failback**

![Poboljšana failback](./media/site-recovery-components/enhanced-failback.png)


- [Dodatne informacije](site-recovery-vmware-to-azure.md#azure-prerequisites) o preduvjetima za Azure portala implementacije.
- [Dodatne informacije](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) o preduvjetima za implementaciju poboljšane klasični portalu.
- [Saznajte više](site-recovery-failback-azure-to-vmware.md) o failback Auzre portalu.
- [Saznajte više](site-recovery-failback-azure-to-vmware-clas- [Learn more](site-recovery-failback-azure-to-vmware-classic.md) about failback in the Auzre portal.sic.md) o failback klasični portalu.

## <a name="replicate-to-azure-hyper-v-vms-not-managed-by-vmm"></a>Repliciranje za Azure: Hyper-V VMs ne upravlja VMM

Moguće je replicirati Hyper-V VMs koji ne upravlja VMM centar sustava za Azure s oporavak web-mjesta na sljedeći način:

- **Pomoću portala za Azure**– kada implementacija oporavak web-mjesta na portalu Azure možete uspjeti putem VMs klasični za pohranu ili upravitelju resursa. [Dodatne informacije](site-recovery-hyper-v-site-to-azure.md).
- **Pomoću portala za klasični**– možete implementirati oporavak web-mjesta na portalu klasični. U ovom implementacije koje možete samo uspjeti putem VMs klasični pohranom na servisu Azure, a ne i Voditelj resursa za pohranu. [Dodatne informacije](site-recovery-hyper-v-site-to-azure-classic.md).

Arhitektura za oba implementacije je slično, osim što:

- Ako morate je implementirati na portalu Azure možete replicirati Voditelj resursa za pohranu i koristiti mreža resursima za povezivanje Azure VMs nakon prebacivanje.
- Postupak implementacije je pojednostavnjeni i više jednostavnih na portalu za Azure.

Evo što vam je potrebno:

- **Račun za Azure**: potreban vam je račun za Microsoft Azure.
- **Azure prostora za pohranu**: morat ćete račun Azure prostora za pohranu za pohranu repliciranu podataka. Na portalu za Azure možete koristiti klasične računa ili računa za pohranu Voditelj resursa. Na portalu klasični možete koristiti samo klasični računa. Repliciranu podaci se pohranjuju u Azure prostora za pohranu i Azure VMs stvaraju kada dođe do prebacivanje.
- **Azure mreže**: morat ćete Azure mreže koji Azure VMs će se povezati s kada ih se stvorene nakon prebacivanje. 
- **Glavno računalo Hyper-v**:, potreban vam je jedan ili više Windows Server 2012 R2 Hyper-V glavnog poslužitelja. Tijekom implementacije oporavak web-mjesta na glavnom računalu ćete instalirajte davatelja za oporavak Azure web-mjesta i agent servisa Microsoft Azure oporavak Services.
- **Hyper-V VMs**:, potreban vam je jedan ili više VMs na poslužitelju Hyper-V glavnog računala. Azure davatelja oporavak web-mjesta i servisa Azure oporavak agent na glavnom računalu Hyper-V tijekom implementacije oporavak web-mjesta. Davatelj koordinate i orchestrates replikacije sa servisom za oporavak web-mjesta s Interneta. Agenta rukuje podataka replikacije podataka putem HTTP 443. Komunikacija od davatelja usluge i agenta su sigurne i šifrirane. Repliciranu podataka u Azure prostora za pohranu i šifriran.

**Općenito arhitekture**

![Hyper-V web-mjesta za Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


- [Dodatne informacije](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) o preduvjetima za Azure portala implementacije.
- [Dodatne informacije](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) o preduvjetima za klasični portala implementacije.



## <a name="replicate-to-azure-hyper-v-vms-managed-by-vmm"></a>Repliciranje za Azure: Hyper-V VMs upravlja VMM

Mogli ponoviti Hyper-V VMs u VMM oblaka za Azure s oporavak web-mjesta na sljedeći način:

- **Pomoću portala za Azure**– kada implementacija oporavak web-mjesta na portalu Azure možete uspjeti putem VMs klasični za pohranu ili upravitelju resursa. [Dodatne informacije](site-recovery-vmm-to-azure.md).
- **Pomoću portala za klasični**– možete implementirati oporavak web-mjesta na portalu klasični. U ovom implementacije koje možete samo uspjeti putem VMs klasični pohranom na servisu Azure, a ne i Voditelj resursa za pohranu. [Dodatne informacije](site-recovery-vmm-to-azure-classic.md).

Arhitektura za oba implementacije je slično, osim što:

- Ako morate je implementirati na portalu Azure možete replicirati utemeljen na Voditelj resursa za pohranu i koristiti resursima mreža o povezivanju sa servisom Azure VMs nakon prebacivanje.
- Postupak implementacije je pojednostavnjeni i više jednostavnih na portalu za Azure.


Evo što vam je potrebno:

- **Račun za Azure**: potreban vam je račun za Microsoft Azure.
- **Azure prostora za pohranu**: morat ćete račun Azure prostora za pohranu za pohranu repliciranu podataka. Na portalu za Azure možete koristiti klasične računa ili računa za pohranu Voditelj resursa. Na portalu klasični možete koristiti samo klasični računa. Repliciranu podaci se pohranjuju u Azure prostora za pohranu i Azure VMs stvaraju kada dođe do prebacivanje.
- **Azure mreže**: morat ćete postaviti mrežu mapiranja tako da se Azure VMs povezani s odgovarajućim mrežama kada ih se stvorene nakon prebacivanje. 
- **VMM poslužitelja**: ćete moraju jedan ili više lokalne VMM poslužitelje pokrenut na sustavu centar 2012 R2 ili postaviti s jednog ili više privatne oblaka. Ako ste implementacija u na Azure portala, potreban vam je logički i VM mreže postavili tako da možete konfigurirati preslikavanje. Na portalu klasični Ovo nije obavezno.  VM mreže mora biti povezan s logičke mreže povezane sa oblaka.
- **Glavno računalo Hyper-v**:, potreban vam je jedan ili više poslužiteljima sustava Windows Server 2012 R2 Hyper-V glavnog računala u oblaku VMM.
- **Hyper-V VMs**:, potreban vam je jedan ili više VMs na poslužitelju Hyper-V glavnog računala.

**Općenito arhitekture**

![VMM za Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

- [Dodatne informacije](site-recovery-vmm-to-azure.md#azure-requirements) o preduvjetima za Azure portala implementacije.
- [Dodatne informacije](site-recovery-vmm-to-azure-classic.md#before-you-start) o preduvjetima za klasični portala implementacije.




## <a name="replicate-to-a-secondary-site-vmware-virtual-machines-or-physical-servers"></a>Repliciranje sekundarne web-mjesto: VMware virtualnim strojevima ili fizičke poslužitelja 

Za replikaciju VMware VMs ili fizičke poslužitelje sekundarnog web-mjesta kao preuzimanje InMage Scout koji je sve obuhvaćeno pretplatom oporavak Azure web-mjesta. Mogu se preuzeti s portala za Azure ili s portala za Azure klasični. 

Postavljanje poslužitelje komponente u svakom web-mjestu (konfiguracije procesa, glavni cilj) i instalirajte Agent za Sjedinjeno komuniciranje na koje želite replicirati. Nakon početnog replikacije agent na svakom računalu šalje delta replikacije promjene na poslužitelj za postupak. Poslužitelj za postupak optimizira podatke i prenosi na poslužitelj za osnovne cilj na sekundarnog web-mjesta. Poslužitelj za konfiguraciju upravlja replikacije postupkom.

Evo što vam je potrebno:

**Račun za Azure**: implementacija scenarij pomoću InMage Scout. Da biste ga nabavili morat ćete Azure pretplate. Nakon stvaranja web-mjesta oporavak sigurnog preuzimanje InMage Scout i instalirajte najnovija ažuriranja za postavljanje uvođenje.
**Postupak server (primarni web-mjesta)**: postavljanje komponentu poslužitelja postupak u primarni web-mjesta za rukovanje predmemoriranja, sažimanja i optimizirati podataka. Bavi i automatske instalacije Agent za Sjedinjeno komuniciranje na računalima koji želite zaštititi. 
**VMware ESX ESXi i vCenter server (primarni web-mjesta)**: Ako ste zaštita VMware VMs morat ćete VMware EXS ESXi hypervisor i po želji VMware vCenter poslužitelju za upravljanje hypervisors.
- **VMs/fizičke poslužitelje (primarni web-mjesta)**: VMware VMs ili Windows/Linux fizičke poslužitelja koji želite zaštititi će potrebno instaliran Agent Sjedinjeno komuniciranje. Agent za Sjedinjeno komuniciranje mora biti instalirana strojeva ulozi osnovne odredišni poslužitelj. Agenta ponaša se kao davatelj komunikacije između sve komponente. 
- - **Poslužitelj za konfiguraciju (sekundarnog web-mjesta)**: poslužitelj za konfiguraciju je prva komponenta instalirate pa je instalirana na sekundarnog web-mjesta za upravljanje, konfiguriranje i praćenje implementaciju sustava ili pomoću upravljanja web-mjesta ili konzole za vContinuum. Postoji samo jedan konfiguraciju poslužitelja u implementaciji, a zatim ga na mora biti instaliran na računalo sa sustavom Windows Server 2012 R2.
- **vContinuum server (sekundarnog web-mjesta)**: je instaliran na istom mjestu (sekundarnog web-mjesta) kao poslužitelj za konfiguraciju. Pruža na konzoli za Upravljanje projektom i nadzor zaštićeni okruženju. U Zadana instalacija vContinuum je li poslužitelj prvi poslužitelj osnovne cilj te je instaliran Agent za Sjedinjeno komuniciranje.
- **Matrica poslužitelju ciljni (sekundarnog web-mjesta)**: osnovne ciljnom poslužitelju sadrži Repliciranu podatke. Ga prima podatke iz poslužitelj za postupak stvara stroj replike u sekundarnog web-mjesta, a sadrži zadržavanja točke podataka. Broj osnovne ciljnim poslužiteljima vam je potrebna ovisi o broju strojeva ste zaštitu. Ako želite da se neće vratiti primarni web-mjesta morate poslužitelja osnovne cilj ima previše. 

**Općenito arhitekture**

![VMware za VMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="replicate-to-a-secondary-site-hyper-v-vms-managed-by-vmm"></a>Repliciranje sekundarne web-mjesto: VMs Hyper-V upravlja VMM


Moguće je replicirati Hyper-V VMs kojima upravlja VMM centar sustava sekundarne podatkovnim centrom s oporavak web-mjesta na sljedeći način:

- **Pomoću portala za Azure**– kada implementacija oporavak web-mjesta na portalu za Azure. [Dodatne informacije](site-recovery-hyper-v-site-to-azure.md).
- **Pomoću portala za klasični**– možete implementirati oporavak web-mjesta na portalu klasični. [Dodatne informacije](site-recovery-hyper-v-site-to-azure-classic.md).

Arhitektura za oba implementacije je slično, osim što:

- Ako pokrenete na portalu za Azure morate postaviti preslikavanje. Ovo nije obavezno klasični portalu.
- Postupak implementacije je pojednostavnjeni i više jednostavnih na portalu za Azure.
- - Ako pokrenete u na Azure klasični portala [za pohranu mapiranje](site-recovery-storage-mapping.md) je dostupna.

Evo što vam je potrebno:

- **Račun za Azure**: potreban vam je račun za Microsoft Azure.
- **VMM poslužitelja**: preporučujemo VMM poslužitelja u primarni web-mjesta i u sekundarni web-mjesta, svaki koja sadrži barem jedan VMM privatne oblaka. Poslužitelj mora imati najmanje sustav centrirali 2012 SP1 s najnovijim ažuriranjima i povezani s Internetom. Oblaka moraju imati profil mogućnost Hyper-V postavljen. Na poslužitelju VMM ćete instalirati davatelja za oporavak Azure web-mjesta. Davatelj koordinate i orchestrates replikacije sa servisom za oporavak web-mjesta s Interneta. Komunikaciju između davatelja usluga i Azure su sigurne i šifrirane.
- **Hyper-V poslužitelja**: Hyper-V glavnog računala poslužitelja mora se nalaziti u VMM oblaka primarnih i sekundarnih. Glavno računalo poslužitelja mora imati najmanje Windows Server 2012 s najnovijim ažuriranjima instaliran i povezani s Internetom. Podaci se replicirati između primarnih i sekundarnih Hyper-V glavnog računala poslužitelja putem LAN-a ili VPN-a pomoću provjere autentičnosti Kerberos ili certifikat.  
- **Zaštićeno strojeva**: izvorni Hyper-V poslužitelj glavno računalo mora imati najmanje jedan VM koji želite zaštititi.

**Općenito arhitekture**

![Lokalni na lokalni](./media/site-recovery-components/arch-onprem-onprem.png)


- [Dodatne informacije](site-recovery-vmm-to-vmm.md#azure-prerequisites) o preduvjetima za implementaciju na portalu za Azure.
- - [Dodatne informacije](site-recovery-vmm-to-vmm-classic.md#before-you-start) o preduvjetima za implementaciju Azure klasični portalu.




## <a name="replicate-to-a-secondary-site-with-san-replication-hyper-v-vms-managed-by-vmm"></a>Repliciranje sekundarne web-mjesto s replikacijom SAN: Hyper-V VMs upravlja VMM

Možete replicirati Hyper-V VMs upravlja iz oblaka VMM sekundarne web-mjesto koristeći SAN replikacije pomoću portala za Azure klasični. Scenarij trenutno nije podržano u novi Azure portal. 

U ovom scenariju tijekom implementacije oporavak web-mjesta na poslužiteljima VMM ćete instalirati davatelja za oporavak Azure web-mjesta. Davatelj koordinate i orchestrates replikacije sa servisom za oporavak web-mjesta s Interneta. Podatke je replicirati između polja za pohranu primarnih i sekundarnih pomoću sinkrono SAN replikacije.

Evo što vam je potrebno:

**Račun za Azure**: morat ćete Azure pretplate
- **Polja SAN**: na [podržani SAN polja](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) upravlja primarni poslužitelj VMM. Na SAN mrežne infrastrukture omogućio drugi SAN polja u sekundarnog web-mjesta.
- **VMM poslužitelja**: preporučujemo VMM poslužitelja u primarni web-mjesta i u sekundarni web-mjesta, svaki koja sadrži barem jedan VMM privatne oblaka. Poslužitelj mora imati najmanje sustav centrirali 2012 SP1 s najnovijim ažuriranjima i povezani s Internetom. Oblaka moraju imati profil mogućnost Hyper-V postavljen.
- **Hyper-V poslužitelja**: Hyper-V glavnog računala poslužitelja koja se nalazi u VMM oblaka primarnih i sekundarnih. Glavno računalo poslužitelja mora imati najmanje Windows Server 2012 s najnovijim ažuriranjima instaliran i povezani s Internetom.
- **Zaštićeno strojeva**: izvorni Hyper-V poslužitelj glavno računalo mora imati najmanje jedan VM koji želite zaštititi.

**Arhitektura SAN replikacije**

![SAN replikacije](./media/site-recovery-components/arch-onprem-onprem-san.png)

[Dodatne informacije](site-recovery-vmm-san.md#before-you-start) o preduvjetima za implementaciju.
### <a name="on-premises"></a>Lokalni



## <a name="hyper-v-protection-lifecycle"></a>Životni ciklus zaštitu Hyper-V

Ovaj tijek rada prikazuje postupak za zaštitu, replikaciju i ne uspijeva putem tehnologije Hyper-V virtualnim računalima. 

1. **Omogući zaštitu**: postavljanje sigurnog oporavak web-mjesta, konfiguriranje postavki replikacije za oblak VMM ili Hyper-V web-mjesta i Omogući zaštitu za VMs. Posao zaštite **Omogućiti** pokreće, a moguće nadzirati na kartici **Zadaci** . Posao provjerava da stroj uspostavu preduvjeti i poziva metodu [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) koja postavlja replikacije za Azure s postavkama ste konfigurirali. **Omogući zaštitu** posao poziva i metodu [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) Inicijalizacija cijelog VM replikacije.
2. **Početni replikacije**: koristi se snimka virtualnog računala i virtualne tvrdi disk su repliciranu jednu po jednu dok je što ste sve kopirane Azure ili sekundarni podatkovnog centra. Vrijeme potrebno da biste dovršili to ovisi o veličina VM, propusnost mreže i metodu početne replikacije. Ako se dogode promjene na disku tijekom početne replikacije praćenja replikacije Hyper-V replike prati te promjene kao Hyper-V replikacije zapisnika (.hrl) koje se nalaze u istu mapu kao i na disk. Svakom disku ima povezani .hrl datoteku koja će se slati sekundarne prostora za pohranu. Imajte na umu da datoteke snimke i prijaviti se trošiti resurse na disku tijekom početne replikacije je u tijeku. Kada se dovrši početne replikacije VM snimka se briše i promjene na disku delta u zapisniku će se sinkronizirati i spojiti.
3. **Zaštita Finalize**: nakon početnog replikacije Završi zadatak **Finalize zaštitu** konfigurira mreža i ostale postavke poslije replikacije tako da je zaštićen virtualnog računala. Ako ste replikaciju za Azure možda ćete morati dotjerati postavki virtualnog računala tako da bude spreman za prebacivanje. Sada možete pokrenuti test prebacivanje da biste provjerili funkcionira sve prema očekivanjima.
4. **Replikacija**: nakon početnog replikacije delta započne sinkronizacija, skladu replikacije postavkama. 
    - **Replikacija nije uspjelo**: Ako delta replikacije ne uspije, i cijeli replikacije bio skup pomoću propusnosti ili vremena, a zatim resynchronization pojavljuje. Za primjer ako datoteka .hrl dođete do 50% veličina diska pa na VM bit će označene za resynchronization. Resynchronization minimizira količinu podataka koji se šalju računalstvo checksums virtualnim strojevima izvorišno i odredišno i slanje samo delta. Po završetku resynchronization delta replikacije će nastaviti. Prema zadanim postavkama resynchronization zakazano automatsko pokretanje izvan sustava office sata, ali ručno ponovno sinkroniziranje virtualnog računala.
    - **Replikacija pogreške**: Ako dođe do pogreške u replikacije nema ugrađenu pokušajte ponovno. Ako je u ne-popravljiva pogreška kao što je pogreška provjere autentičnosti ili autorizacije ili replike računalo je u stanju koje nije valjano, pa nema pokušaj će se pokušati izvesti. Ako je popravljiva pogreška, kao što je mrežne pogreške ili razmak memorije disku, a zatim na pokušaj pojavljuje povećanim intervala između ponovne pokušaje (1, 2, 4, 8, 10, a zatim svakih 30 minuta).
4. **Planirano neplanirano failovers**: možete pokrenuti planiranog ili neplanirano failovers prema potrebi. Ako ste pokrenuli planiranog prebacivanje zatim izvora VMs su Zatvori da biste bili sigurni bez gubitka podataka. Nakon stvaranja replike VMs se smješta u potvrdi na čekanju stanje. Morate izvršiti ih da biste dovršili za prebacivanje, osim ako ste replikaciju s SAN u tom slučaju potvrdi odvija se automatski. Po završetku primarni web-mjesta s radom failback može se pojaviti. Ako ste replicirati na Azure obrnutim replikacije odvija se automatski. U suprotnom vam izbaciti obrnutim replikacije ručno.
 

![tijek rada](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Daljnji koraci

[Priprema za implementaciju](site-recovery-best-practices.md)
