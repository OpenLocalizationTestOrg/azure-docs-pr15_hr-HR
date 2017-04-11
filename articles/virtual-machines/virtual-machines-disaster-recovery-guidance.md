<properties
    pageTitle="Što učiniti u slučaju da je prekidu Azure service utječe Azure virtualnim strojevima | Microsoft Azure"
    description="Saznajte što učiniti u slučaju da je prekidu Azure service utječe Azure virtualnim strojevima."
    services="virtual-machines"
    documentationCenter=""
    authors="kmouss"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="virtual-machines"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-virtual-machines"></a>Što učiniti u slučaju da je prekidu Azure service utječe Azure virtualnim strojevima

Microsoft, radimo konačnog da biste bili sigurni da naših usluga uvijek su dostupne kada su vam potrebne. Prisilno izvan našu kontrola nam ponekad utjecati na način koji uzrokuju to izbjeglo neplanirano servisa.

Microsoft pruža usluge ugovor o razini (SLA) za njegove servise kao izvršenja za vrijeme aktivnosti i povezivanjem. SLA za pojedinačne Azure usluge pronaći ćete na [Ugovore o razini usluge za Azure](https://azure.microsoft.com/support/legal/sla/).

Azure već ima mnoge značajke ugrađene platformu koji podržavaju iznimno dostupnih aplikacija. Dodatne informacije o tih servisa, pročitajte [oporavak Izrada i visoke dostupnosti za Azure aplikacije](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

U ovom se članku pokriva true Izrada scenarij za oporavak kada cijelog područja dođe do prekida zbog glavne prirodnim Izrada ili prekid Primamo servisa. To su rijetko pojavljivanja, ali morate pripremiti za mogućnost ima li se prekida cijelo područje. Ako cijelo područje sučelja servisa prekidu, lokalno suvišnih kopije vaših podataka bi privremeno nedostupan. Ako ste omogućili zemlj replikacije, tri primjerka blob polja za pohranu Azure i tablice spremaju se u nekoj drugoj regiji. U slučaju dovršeno regionalne prekida ili Izrada u kojem primarni regija nije oporaviti, Azure remaps sve DNS unose s područjem zemlj replicirati.

>[AZURE.NOTE]Imajte na umu da nemate sve kontrole nad taj postupak, a će javiti samo za to izbjeglo regija razini usluge. Zbog toga, morate je za druge strategije specifičnim aplikacijama sigurnosnu kopiju da biste postigli najvišu razinu dostupnost. Dodatne informacije potražite u odjeljku [Strategije podatke za oporavak Izrada](../resiliency/resiliency-disaster-recovery-azure-applications.md#data-strategies-for-disaster-recovery).

Da biste lakše rukovati tih rijetko pojava, dajemo sljedeće smjernice za Azure virtualnim strojevima slučaju servisa prekidu cijelog područja gdje je implementiran aplikacije Azure virtualnog računala.

##<a name="option-1-wait-for-recovery"></a>Mogućnost 1: Čekanja za oporavak
U ovom slučaju ne poduzmete na svoj dio potreban je. Znate da radimo održavate da biste vratili dostupnost usluge. Vidjet ćete trenutno stanje servisa na našem [Nadzorna ploča za stanje servisa Azure](https://azure.microsoft.com/status/).

>[AZURE.NOTE]To je najbolja mogućnost ako ste postavili oporavak Azure web-mjesta, sigurnosno kopiranje virtualnog računala, pristup za čitanje zemlj suvišnih prostora za pohranu ili zemlj suvišnih prostora za pohranu prije no što u prekidu. Ako ste postavili zemlj suvišnih prostora za pohranu ili zemlj suvišnih prostor za pohranu pristup za čitanje za račun za pohranu pohranjuju VM virtualne teško pogonima (VHDs), možete je izgleda da biste oporavili osnovni slika VHD i pokušajte Dodjela nove VM iz nje. Ovo nije željenu mogućnost jer nema jamstva sinkronizacije podataka osim ako se koriste Azure VM sigurnosno kopiranje ili oporavak Azure web-mjesta. Zbog toga ta mogućnost ne zajamčiti rad.

Za korisnike koji žele izravan pristup virtualnim strojevima, dostupne su sljedeće dvije mogućnosti.  

>[AZURE.NOTE]Imajte na umu i sljedeće mogućnosti imate mogućnost gubitka podataka.     

##<a name="option-2-restore-a-vm-from-a-backup"></a>Mogućnost 2: Vratiti na VM iz sigurnosne kopije
Za korisnike koji ste konfigurirali VM sigurnosnu kopiju, možete vratiti na VM od točke i oporavak sigurnosne kopije.

Da biste vratili novi VM iz sigurnosne kopije Azure, potražite u članku [Vraćanje virtualnim strojevima u Azure](../backup/backup-azure-restore-vms.md).

Da biste lakše isplanirali za vaše Azure virtualnim strojevima infrastruktura za sigurnosne kopije, potražite u članku [Planiranje preduvjete VM sigurnosne kopije infrastrukture u Azure](../backup/backup-azure-vms-introduction.md).

##<a name="option-3-initiate-a-failover-by-using-azure-site-recovery"></a>Mogućnost 3: Pokretanje programa prebacivanje pomoću oporavak Azure web-mjesta
Ako ste konfigurirali Azure oporavak web-mjesta da biste radili s impacted Azure virtualnim računalima, možete se vratiti na VMs iz njihove replike. Ove replike se mogu nalaziti na Azure ili lokalno. U tom slučaju možete stvoriti novu VM iz njegove postojeće replike. Da biste vratili na VMs iz programa replike oporavak Azure web-mjesta, potražite u članku [Migracija Azure IaaS virtualnim strojevima među Azure područjima s oporavak Azure web-mjesta](../site-recovery/site-recovery-migrate-azure-to-azure.md).

>[AZURE.NOTE]Iako Azure virtualnog računala operacijski sustav i diskova podataka će je replicirati na sekundarne VHD, ako se ona nalazi u zemlj suvišnih prostora za pohranu ili račun za pristup za čitanje zemlj suvišnih prostora za pohranu, svaki VHD je replicirati zasebno. Ta razina replikacije ne jamči dosljednost preko repliciranu VHDs. Ako vašeg računala i/ili baze podataka koje koriste tih podataka diskova ovisnosti na drugu, ga se ne zajamčiti da se svi VHDs replicirati kao jedan snimku. Ga je i nije zajamčiti replike VHD iz zemlj suvišnih prostora za pohranu ili pristup za čitanje zemlj suvišnih prostora za pohranu će rezultirati aplikacije dosljedan snimku za pokretanje sustava VM.

##<a name="next-steps"></a>Daljnji koraci
Da biste saznali više o tome kako implementirati visoke dostupnosti strategije i oporavak Izrada, potražite u članku [oporavak Izrada i visoke dostupnosti za Azure aplikacije](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Razvoj detaljne tehničke razumijevanje mogućnosti platformu oblaka, potražite u članku [Azure otpornost Tehnički vodič](../resiliency/resiliency-technical-guidance.md).

Da biste saznali kako se sigurnosno kopiranje VMs, potražite u članku [sigurnosno kopiranje Azure virtualnih računala](../backup/backup-azure-vms.md).

Saznajte kako koristiti oporavak web-mjesta Azure orkestrirali i automatizaciju zaštite vaše fizičke (i virtualne) sustava Windows i Linux strojeva koji se pokreću na VMWare i Hyper-V VMs potražite u članku [Oporavak Azure web-mjesta](https://azure.microsoft.com/documentation/learning-paths/site-recovery/).

Ako su upute ne isključite ili ako želite da na vaše ime učiniti, obratite se [Službi](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
