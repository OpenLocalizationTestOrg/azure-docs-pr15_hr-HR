<properties
    pageTitle="Azure oporavak web-mjesta: Najčešća pitanja | Microsoft Azure"
    description="U ovom se članku govori o najčešćih pitanja o oporavak Azure web-mjesta."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="get-started-article"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>


# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure oporavak web-mjesta: Najčešća pitanja

Ovaj članak sadrži najčešća pitanja o oporavak Azure web-mjesta. Ako imate pitanja kad pročitate članak, postavite ih na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Općenito

### <a name="what-does-site-recovery-do"></a>Čemu služi oporavak web-mjesta?

Oporavak web-mjesta doprinosa tvrtke continuity i Izrada oporavak (BCDR) strategije, orchestrating i automatske replikacije iz lokalnog virtualnim strojevima i fizičke poslužitelje Azure ili sekundarni podatkovnog centra. [Dodatne informacije](site-recovery-overview.md).


### <a name="what-can-site-recovery-protect"></a>Što štiti oporavak web-mjesta?

- **Hyper-V virtualnim strojevima**: oporavak web-mjesta možete zaštititi sve radno opterećenje sustavom Hyper-V VM.
- **Fizička poslužitelji**: oporavak web-mjesta možete zaštititi fizičke poslužiteljima sa sustavom Windows ili Linux.
- **VMware virtualnim strojevima**: oporavak web-mjesta možete zaštititi sve radno opterećenje izvodi u VMware VM.

### <a name="does-site-recovery-support-the-azure-resource-manager-model"></a>Podržava li oporavak web-mjesta u model upravljanja resursima Azure?

Osim oporavak web-mjesta na portalu za Azure klasični oporavak web-mjesta je dostupna na portalu za Azure s podrškom za Voditelj resursa. Većini implementacije scenarija oporavak web-mjesta u na Azure portal nudi iskustvo pojednostavnjeno uvođenja i replicirati VMs i fizičke poslužitelji u klasični prostora za pohranu ili Voditelj resursa za pohranu. Evo podržanih implementacije:

- [Replicirati VMware VMs ili fizičke poslužitelji za Azure na portalu za Azure](site-recovery-vmware-to-azure.md)
- [Replicirati Hyper-V VMs u VMM oblaka za Azure na portalu za Azure](site-recovery-vmm-to-azure.md)
- [Replicirati VMs Hyper-V (bez VMM) za Azure na portalu za Azure](site-recovery-hyper-v-site-to-azure.md)
- [Replicirati Hyper-V VMs u VMM oblaka za sekundarnog web-mjesta na portalu za Azure](site-recovery-vmm-to-vmm.md)


### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Što je potrebno u Hyper-V tako da biste orkestrirali replikacije s oporavak web-mjesta?

Glavnog računala poslužitelja Hyper-V tako što vam je potrebna ovisi o scenarij implementacije. Pogledajte preduvjete Hyper-V u:

- [Replikaciju VMs Hyper-V (bez VMM) za Azure](site-recovery-hyper-v-site-to-azure.md#before-you-start)
- [Replikaciju VMs Hyper-V (s VMM) za Azure](site-recovery-vmm-to-azure.md#before-you-start)
- [Hyper-V VMs replikaciju sekundarnu podatkovnim centrom](site-recovery-vmm-to-vmm.md#before-you-start)

- Ako ste replikaciju sekundarnu podatkovnim centrom Saznajte više o [podržani goste operacijski sustavi za VMs Hyper-V](https://technet.microsoft.com/library/mt126277.aspx).
- Ako ste replikaciju za Azure, oporavak web-mjesta podržava sve goste operacijskim sustavima koji su [podržava Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Može li se zaštiti VMs kada Hyper-V radi u operacijskom sustavu klijenta?

Ne, VMs mora biti smješteno na poslužitelju Hyper-V glavno računalo sa sustavom na podržani računalo Windows server. Ako vam je potrebna zaštita klijentsko računalo nije moguće je replicirati kao fizičke strojno [Azure](site-recovery-vmware-to-azure.md) ili [sekundarni podatkovnog centra](site-recovery-vmware-to-vmware.md).


### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Što radnih opterećenja možete zaštititi pomoću oporavak web-mjesta?

Oporavak web-mjesta možete koristiti da biste zaštitili Većina radnih opterećenja izvodi na podržani VM ili fizičke poslužitelja. Oporavak web-mjesta pruža podršku za aplikaciju umu replikacije tako da se može oporaviti aplikacije Inteligentna stanje. Integrira aplikacije Microsoft kao što je SharePoint, Exchange, Dynamics, SQL Server i servisa Active Directory i usko surađuje s početne dobavljačima, uključujući Oracle, SAP, IBM i crveno razgovor. [Dodatne informacije](site-recovery-workload.md) o zaštiti radno opterećenje.


### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Hyper-V domaćini treba biti u VMM oblaka?

Ako želite za replikaciju sekundarnu podatkovnog centra, a zatim Hyper-V VMs mora se nalaziti na Hyper-V hostira poslužitelja koja se nalazi u VMM oblaka. Ako želite replicirati Azure VMs možete replicirati na Hyper-V glavno računalo poslužiteljima sa ili bez VMM oblaka. [Dodatne informacije potražite u](site-recovery-hyper-v-site-to-azure.md).

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Možete implementirati oporavak web-mjesta s VMM ako imam samo jedan VMM poslužitelj?

Da. VMs mogli ponoviti ili na poslužiteljima Hyper-V u oblaku VMM za Azure ili mogli ponoviti između VMM oblaka na istom poslužitelju. Informacije o lokalnom za replikaciju lokalnog, preporučujemo da imate VMM poslužitelja u oba primarnih i sekundarnih web-mjesta.  [Saznajte više](site-recovery-single-vmm.md)

### <a name="what-physical-servers-can-i-protect"></a>Što fizičke poslužitelja možete zaštititi?

Možete replicirati fizičke poslužitelje s programom Windows i Linux Azure ili sekundarnog web-mjesta. [Informirajte se o](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) sistemske preduvjete.  Preduvjeti za istu primjenjuju bez obzira koristite replikaciju fizičke poslužitelje Azure ili sekundarnog web-mjesta.

Imajte na umu da fizičke poslužitelje će pokrenuti kao VMs u Azure ako funkcionira lokalnog poslužitelja. Failback za lokalni poslužitelj fizičke trenutno nije podržano, no uspijeva natrag na virtualnog računala koja se izvodi na Hyper-V ili VMware.


### <a name="what-vmware-vms-can-i-protect"></a>Što VMs VMware možete zaštititi?

Da biste zaštitili VMware VMs morat ćete vSphere hypervisor i virtualnim strojevima pokrenut VMware Alati. Preporučujemo i da imate VMware vCenter poslužitelju za upravljanje u hypervisors. [Saznajte više](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) o točno preduvjetima za replikaciju VMware poslužitelji i VMs Azure ili sekundarnog web-mjesta.

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Možete upravljati Izrada oporavak za moj podružnicama s oporavak web-mjesta?

Da. Kada koristite oporavak web-mjesta da biste orkestrirali replikaciju i prebacivanje u vašem podružnicama, dobit ćete objedinjenih djelovanje i prikazu sve svoje granu office radnih opterećenja na središnjem mjestu. Jednostavno možete pokrenuti failovers i upravljati oporavak Izrada sve grana iz svog ureda glavni bez posjeta se grana.

## <a name="security"></a>Sigurnost

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Replikacija podataka šalju na servis za oporavak web-mjesta?

Ne, ne intercept repliciranu podataka oporavak web-mjesta, a ne sadrži podatke o radi na virtualnim strojevima ili fizičke poslužiteljima.
Replikacija podataka je razmjenjivati između lokalnog Hyper-V domaćini, VMware hypervisors ili fizičke poslužitelji i Azure prostora za pohranu ili sekundarnog web-mjesta. Oporavak web-mjesta sadrži nema mogućnost intercept tih podataka. Samo metapodataka potrebne za orkestrirali replikaciju i prebacivanje šalje se na servis za oporavak web-mjesta.

Oporavak web-mjesta je ISO 27001:2013, 27018, HIPAA DPA certificirane te je u tijeku SOC2 i FedRAMP JAB konfiguracije.


### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Usklađenost razloga, čak i za naše metapodataka lokalnog mora biti unutar iste regiji. Oporavak web-mjesta omogućuju nam?

Da. Kada stvorite zbirke ključeva za oporavak web-mjesta u regiji, ne možemo provjerite je li sve metapodatke koje je potrebno je omogućiti i orkestrirali replikacije i prebacivanje ostaje unutar te regije geografske granicu.

### <a name="does-site-recovery-encrypt-replication"></a>Ne oporavak web-mjesta šifrirati replikacije?

Virtualnim strojevima i fizičke poslužitelje, replikaciju između lokalnog web-mjesta šifriranje-tranzitne podržana. Za virtualnim strojevima i fizičke poslužitelje replikaciju Azure, podržani su šifriranje tranzitne i šifriranje-na-ostale (u Azure).


## <a name="replication"></a>Replikacije


### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure"></a>Postoje li sve preduvjete za replikaciju virtualnim strojevima za Azure?

Virtualnim strojevima želite replicirati Azure moraju biti usklađene sa [Azure preduvjeti](site-recovery-best-practices.md#virtual-machines).

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Li replicirati Hyper-V generiranja 2 virtualnim strojevima za Azure?

Da. Oporavak web-mjesta za generiranje 1 tijekom prebacivanje pretvara iz generacije 2. Pri failback računalu pretvorit će vratiti generacije 2. [Dodatne informacije potražite u](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms"></a>Ako se replicirati Azure kako učinite li platiti Azure VMs?

Tijekom običnog replikacije podataka je replicirati na zemlj suvišnih Azure prostora za pohranu i ne morate platiti naknade za virtualnog računala sve Azure IaaS pruža značajnu iskoristiti. Kada pokrenete u prebacivanje u Azure, oporavak web-mjesta automatski stvara Azure IaaS virtualnim strojevima, a nakon toga koje će naplatiti za resurse računalnim koji zauzimaju u Azure.


### <a name="is-there-an-sdk-i-can-use-to-automate-the-asr-workflow"></a>Je li SDK-a možete koristiti da biste automatizirali ASR tijeka rada?

Da. Možete automatizirati tijekove rada oporavak web-mjesta pomoću Rest API-JA, PowerShell ili Azure SDK. Trenutno podržava scenariji za implementaciju oporavak web-mjesta pomoću komponente PowerShell:

- [Azure PowerShell klasični replicirati Hyper-V VMs u VMMs oblaka](site-recovery-deploy-with-powershell.md)
- [Replicirati Hyper-V VMs u VMMs oblaka za Voditelj resursa PowerShell Azure](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Replicirati Hyper-V VMs bez VMM Azure PowerShell klasični](site-recovery-hyper-v-site-to-azure-classic.md)
- [Replicirati Hyper-V VMs bez VMM s upraviteljem Azure PowerShell resursa](site-recovery-deploy-with-powershell-resource-manager.md)


### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-do-i-need"></a>Ako se replicirati za Azure Kakvu vrstu računa za pohranu potrebno?

- **Azure klasični portal**: Ako ste implementacija oporavak web-mjesta na portalu za Azure klasični, potreban vam je [račun standardni zemlj suvišnih prostora za pohranu](../storage/storage-redundancy.md#geo-redundant-storage). Prostor za pohranu Premium trenutno nije podržano. Račun mora biti u području isti kao sigurnog oporavak web-mjesta.

- **Portal za Azure**: Ako ste implementacija oporavak web-mjesta na portalu za Azure, morat ćete je račun za pohranu LRS ili GRS. Preporučujemo da GRS tako da se podaci prebacuju ako regionalne nestalo ili ako je primarni regija nije moguće oporaviti. Račun mora biti u području isti kao sigurnog servise za oporavak. Prostor za pohranu Premium podržana je samo ako ste replikaciju VMware VMs ili fizičke poslužiteljima.

### <a name="how-often-can-i-replicate-data"></a>Učestalost ponavljanja podataka?

- **Hyper V:** Hyper-V VMs moguće je replicirati svakih 30 sekundi pet minuta ili 15 minuta. Ako ste postavili SAN replikacije pa replikacije je slika.
- **VMware i fizičke poslužitelja:** Replikacija učestalost odgovarajući nije ovdje. Replikacija nije neprekinuto.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Mogu li se proširiti replikacije iz postojećeg oporavak web-mjesta na drugo web-mjesto tertiary?

Znakove ili ulančana replikacija nije dostupna. Zahtjev za tu značajku [forum za povratne informacije](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).


### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Učiniti izvanmrežnu reprodukciju prvi put li replicirati Azure?

To nije podržano. Zahtjev za tu značajku [forum za povratne informacije](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).


### <a name="can-i-exclude-specific-disks-from-replication"></a>Li izuzeli određene diskova od replikacije?

To je sve podržano kad ste [replikaciju VMware VMs i fizičke poslužitelji](site-recovery-vmware-to-azure.md#exclude-disks-from-replication) za Azure pomoću portala za Azure.


### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Mogu li replicirati virtualnim strojevima sa dinamičkih diskova?

Kada replikaciju Hyper-V virtualnim strojevima podržani su dinamičkih diskova. I su podržane kada replikaciju VMware VMs i fizičke strojeva Azure. Disk operacijski sustav mora biti osnovni disk.

### <a name="can-i-add-a-new-machine-to-an-existing-replication-group"></a>Kako dodati novo računalo u postojeću grupu replikacije?

Dodavanje novog strojeva postojećim grupama replikacije je podržano. Da biste to učinili, odaberite grupu replikacije (iz plohu "Replicated stavke") i desnom tipkom miša kliknite/odabir Kontekstni izbornik u grupi replikacije, a zatim odaberite odgovarajuću mogućnost.

![Dodavanje u grupu replikacije](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Mogu li throttle propusnosti dodijeljenom za promet replikacije Hyper-V?

Da. Dodatne informacije o ograničavanje propusnosti u člancima implementacije:

- [Planiranje za replikaciju VMware VMs i fizičke poslužitelje kapaciteta](site-recovery-vmware-to-azure.md#step-5-capacity-planning)
- [Planiranje za replikaciju Hyper-V VMs u VMM oblaka kapaciteta](site-recovery-vmm-to-azure.md#step-5-capacity-planning)
- [Planiranje za replikaciju Hyper-V VMs bez VMM kapaciteta](site-recovery-hyper-v-site-to-azure.md#step-5-capacity-planning)

## <a name="failover"></a>Prebacivanje


### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover"></a>Ako mi se ne uspijeva putem Azure, kako pristupiti Azure virtualnim strojevima nakon prebacivanje?

Azure VMs možete pristupiti putem sigurne internetske veze, putem web-mjesto VPN-a ili putem Azure ExpressRoute. Morat ćete pripremiti mnogo radi povezivanja. Dodatne informacije u:

- [Povezivanje s Azure VMs nakon prebacivanje VMware VMs ili fizičke poslužitelja](hsite-recovery-vmware-to-azure.md#step-7-test-the-deployment)
- [Povezivanje s Azure VMs nakon prebacivanje Hyper-V VMs u VMM oblaka](site-recovery-vmm-to-azure.md#step-7-test-your-deployment)
- [Povezivanje s Azure VMs nakon prebacivanje Hyper-V VMs bez VMM](site-recovery-hyper-v-site-to-azure.md#step-7-test-the-deployment)


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Ako se neće putem Azure kako Azure provjerite je li Moji podaci se prebacuju?

Azure namijenjen je resilience. Oporavak web-mjesta već je izgrađen za prebacivanje na sekundarnom Azure podatkovnim centrom, skladu Azure SLA ako potreba. Ako se to dogodi, ne možemo provjerite je li vaša metapodataka i sefovi ostaju unutar iste geografske područja koje ste odabrali za vaše zbirke ključeva.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Ako se sam replikaciju između dva podatkovnim centrima što se događa ako moj primarni podatkovnog centra iskustvo neočekivane nedostupnosti?

Možete pokrenuti neplanirano prebacivanje iz sekundarnog web-mjesta. Oporavak web-mjesta nisu potrebna povezivanja s primarni web-mjesta da biste izvršili u prebacivanje.

### <a name="is-failover-automatic"></a>Je li automatsko prebacivanje?

Prebacivanje nije automatski. Pokretanje failovers s jednim klikom na portalu ili koristite [PowerShell oporavak web-mjesta](https://msdn.microsoft.com/library/dn850420.aspx) za pokretanje programa prebacivanje. Neispravni ponovno je jednostavno akcije na portalu za oporavak web-mjesta.

Da biste automatizirali koje nije moguće pomoću lokalnog Orchestrator ili Operations Manager da biste otkrili pogreške virtualnog računala, a pokretanje prebacivanje pomoću SDK-a.

- [Dodatne informacije potražite u](site-recovery-create-recovery-plans.md) o tarifama za oporavak.
- [Dodatne informacije potražite u](site-recovery-failover.md) prebacivanje.
- [Dodatne informacije potražite u](site-recovery-failback-azure-to-vmware.md) neuspjeh sigurnosno VMware VMs i fizičke poslužitelja


## <a name="service-providers"></a>Davatelji usluga


### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Ja sam davatelja usluga. Oporavak web-mjesta radi namjenski i zajedničke infrastrukture modela?

Da, oporavak web-mjesta podržava oba modelima namjenski i zajedničke infrastrukture.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Za davatelja usluga, je identiteta klijent zajednički se koristi sa servisom za oporavak web-mjesta?

ne. Anonimni ostaje identiteta klijenta. Vaš klijenata ne mora imati pristup portalu oporavak web-mjesta. Samo administrator davatelja usluge stupi u interakciju s portala sustava.


### <a name="will-tenant-application-data-ever-go-to-azure"></a>Klijentske aplikacije podataka ikad idu u Azure?

Kada replikaciju između web-mjesta vlasništvo davatelja usluge, podataka aplikacije nikad odlazak Azure. Podaci šifriraju tranzitne i repliciranu izravno između web-mjesta davatelja usluga.

Ako ste replikaciju za Azure, podaci za aplikacije se šalju Azure prostora za pohranu, ali ne i servis za oporavak web-mjesta. Podaci šifrirane tranzitne, i ostaje šifrirane u Azure.


### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Primit će klijenata za moj račun za Azure servisi?

ne. Azure na naplatu odnos je izravno kod davatelja usluga. Davatelji usluga ste odgovorni za generiranje određene računi za svoje drugih korisnika.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Ako se replikaciju za Azure, potrebno da biste pokrenuli virtualnim strojevima u Azure stalno?

Ne, podataka je replicirati na račun za Azure prostora za pohranu u pretplatu. Prilikom izvršavanja test prebacivanje (DR analize) ili stvarni prebacivanje oporavak web-mjesta automatski stvara virtualnim strojevima u svoju pretplatu.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Želite provjeriti klijentu razinu odvajanja kada se replicirati Azure?

Da.

### <a name="what-platforms-do-you-currently-support"></a>Koje platforme koje trenutno podržava?

Podržavamo Azure paket, oblaka platformu sustava, a centar sustava temelji implementacije (2012 i noviji). [Saznajte više](https://technet.microsoft.com/library/dn850370.aspx) o Integracija paket Azure i oporavak web-mjesta.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Ne podržavaju jedan Azure za komercijalni ispis jedne implementacije VMM server?

Da, replicirati Hyper-V virtualnim strojevima za Azure ili replicirati između web-mjesta davatelja usluga.  Imajte na umu da ako replicirati između web-mjesta davatelja usluge, Integracija Azure runbook nije dostupna.


## <a name="next-steps"></a>Daljnji koraci

- Pročitajte [Pregled oporavak web-mjesta](site-recovery-overview.md)
- Dodatne informacije o [arhitektura oporavak web-mjesta](site-recovery-components.md)  
