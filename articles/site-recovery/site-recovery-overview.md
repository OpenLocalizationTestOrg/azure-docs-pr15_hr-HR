<properties
    pageTitle="Što je oporavak web-mjesta? | Microsoft Azure"
    description="Sadrži pregled servisa Azure oporavak web-mjesta, a navedene su scenariji za implementaciju."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/13/2016"
    ms.author="raynew"/>

#  <a name="what-is-site-recovery"></a>Što je oporavak web-mjesta?

Dobro došli u web-mjesta za Azure oporavak! Ovaj članak sadrži kratak pregled servis za oporavak web-mjesta i kako doprinosa pridonijeti vašem poslovanju.

Tvrtka ili ustanova mora poslovanje i Izrada oporavak (BCDR) strategije koje su aplikacije, radnih opterećenja i podataka sigurnih i dostupne tijekom planirane i Neplanirana nedostupnost i čim oporavlja normalno funkcionira uvjeta. Oporavak web-mjesta je Azure service doprinosa ovaj strategije.

Oporavak web-mjesta orchestrates replikacije radnih opterećenja lokalne poslužitelje fizičke i virtualnih računala. Možete replicirati poslužitelji i VMs iz primarnog podatkovnog centra u oblak (Azure) ili sekundarni podatkovnim centrom. Prilikom kvarove u primarni web-mjesta, koji se neće putem sekundarnog web-mjesta da biste zadržali aplikacije i radnih opterećenja dostupna i. Ne natrag na svom primarnom mjestu kada on se vraća na običnom operacije.

## <a name="site-recovery-in-the-azure-portal"></a>Oporavak web-mjesta na portalu za Azure

Azure sadrži dvije različite [implementacije modela](../resource-manager-deployment-model.md) za stvaranje i rad s resursima. Model upravljanja resursima Azure i model upravljanja klasičnom usluga. Azure ima dvije portalima – [Azure klasični portal](https://manage.windowsazure.com/) podržava model klasični implementacije i [Azure portal](https://portal.azure.com) s podrškom za na klasični i resursima modela.

- Oporavak web-mjesta dostupan je u klasični portal i Azure portal.
- Azure klasični portalu podržavaju oporavak web-mjesta s modelom upravljanja klasičnom usluga.
- Na portalu Azure podržavaju klasični modela ili implementacijama Voditelj resursa. 

Informacije u ovom članku odnose se na klasične i Azure portala implementacije. Razlike su zabilježene gdje je to primjenjivo.


## <a name="why-deploy-site-recovery"></a>Zašto implementacija oporavak web-mjesta?

Evo što oporavak web-mjesta možete učiniti za svoju tvrtku.

- **Pojednostavnite BCDR**– možete rukovati replikacije, prebacivanje i oporavak više radnih opterećenja na jednom mjestu na portalu za Azure. Oporavak web-mjesta orchestrates replikaciju i prebacivanje, ali ne intercept podataka aplikacije ili bilo kakve informacije o njemu.
- **Prilagodljivo replikacije osiguraj**– pomoću oporavak web-mjesta mogu replicirati radnih opterećenja sustavom podržani Hyper-V VMs, VMware VMs i fizičke poslužitelji Windows/Linux.
- **Testiranje Izvedi jednostavno replikacije**– oporavak web-mjesta omogućuje failovers test za podršku bušilice oporavak Izrada, bez utjecaja radnog okruženja.
- **Neće uspjeti pokazivač, a zatim Oporavi**– možete pokrenuti planiranog failovers za očekivani kvarove nula podataka gubitka ili neplanirano failovers s minimalnim gubitka podataka ubuduće (ovisno o učestalost ponavljanja) za neočekivane disasters. Nakon prebacivanje, možete ga failback primarni web-mjestima. Oporavak web-mjesta omogućuje oporavak tarife koje mogu sadržavati skripte i radnim knjigama Azure Automatizacija, tako da možete prilagoditi prebacivanje i oporavak aplikacija više razina.
- **Uklanjanje sekundarne podatkovnog centra**– možete replicirati radnih opterećenja Azure, umjesto sekundarnog web-mjesta. Time se uklanja trošak i složenosti održavanje sekundarne podatkovnog centra. Repliciranu podaci se pohranjuju u spremište Azure s resilience koji omogućuje. Kada se pojavi prebacivanje VMs stvaraju se repliciranu podacima.
- **Integrate s postojećeg tehnologijama BCDR**– oporavak web-mjesta integrira s druge značajke BCDR. Na primjer, možete koristiti oporavak web-mjesta da biste zaštitili pozadinskog sustava SQL Server od tvrtke radnih opterećenja, uključujući Ugrađena podrška za SQL Server AlwaysOn da biste upravljali Prebacivanje grupe dostupnosti.

## <a name="what-can-i-replicate"></a>Što mogu replicirati?

Ovo je sažetak mogli ponoviti pomoću oporavak web-mjesta.

![Lokalni na lokalni](./media/site-recovery-overview/asr-overview-graphic.png)

**PONAVLJANJA** | **REPLICIRATI NA** 
---|---
Radnih opterećenja izvode na lokalni VMware VMs | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Sekundarni web-mjesta](site-recovery-vmware-to-vmware.md)
Radnih opterećenja izvode na lokalni Hyper-V VMs upravlja u VMM oblaka  | [Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Sekundarni web-mjesta](site-recovery-vmm-to-vmm.md) 
Radno opterećenje izvode na lokalni Hyper-V VMs upravlja u VMM oblaka, SAN prostora za pohranu|  [Sekundarni web-mjesta](site-recovery-vmm-san.md)
Radnih opterećenja izvode na lokalni Hyper-V VMs bez VMM | [Azure](site-recovery-hyper-v-site-to-azure.md)
Radnih opterećenja sustavom lokalne poslužitelje fizičke Windows/Linux | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Sekundarni web-mjesta](site-recovery-vmware-to-vmware.md)


## <a name="what-workloads-can-i-protect"></a>Što radnih opterećenja možete zaštititi?

Oporavak web-mjesta omogućuje aplikacije umu BCDR tako da se i dalje da biste pokrenuli dosljedno kada se dogodi kvarove radnih opterećenja i aplikacije. Oporavak web-mjesta omogućuje:

- **Aplikacija dosljedan snimaka**– strojeva replicirati pomoću aplikacije dosljedan snimke za jednu ili više razina aplikacije. Uz dohvaćanje podataka na disku, hvatanje aplikacije dosljedan snimke snimite sve podatke u memoriji i sve transakcije u tijeku.
- **Pri sinkronizirano s replikacijom**– oporavak web-mjesta omogućuje učestalost ponavljanja nedostaje kao 30 sekundi da Hyper-V i neprekinuti replikacije za VMware.
- **Prilagodljivo oporavak tarife**– možete stvoriti i prilagoditi tarife za oporavak s vanjskim skripte i ručno akcije. Integracija s runbooks Azure automatizaciju omogućuju vam da biste oporavili u stogu cijelu aplikaciju jednim klikom.
- **Integracija sa SQL Server AlwaysOn**– Prebacivanje grupe dostupnosti možete upravljati u tarifama za oporavak oporavak web-mjesta.
- **Automatizacija biblioteke**– obogaćenog Azure Automatizacija biblioteka omogućuje podrška za proizvodnju, specifičnim aplikacijama skripte koje se mogu preuzeti i integriran s oporavak web-mjesta.
- **Jednostavan mreži upravljanje**– upravljanje Napredne mrežne oporavak web-mjesta i Azure pojednostavljuje aplikacije mreže, uključujući rezervirati IP adrese, konfiguriranje učitavanja balancers i integracija Azure promet upravitelja za switchovers učinkovitog mreže.


## <a name="next-steps"></a>Daljnji koraci

- Pročitajte više u [koje radnih opterećenja oporavak web-mjesta štiti?](site-recovery-workload.md)
- Dodatne informacije o arhitektura oporavak web-mjesta u [oporavak web-mjesta rad s?](site-recovery-components.md)
 
