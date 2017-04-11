<properties
   pageTitle="Tehnički vodič: oporavak lokalnim Azure | Microsoft Azure"
   description="Članak na web-mjesto razumijevanje i u okvir za dizajniranje oporavak sustavi iz lokalnog infrastrukture za Azure"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-on-premises-to-azure"></a>Azure otpornost Tehnički vodič: oporavak lokalnim Azure

Azure nudi opsežan skup servise za omogućivanje proširenje lokalnog podatkovnog centra za Azure za visoke dostupnosti i Izrada oporavak:

* __Povezivanje s mrežom__: S virtualne privatne mreže sigurno proširite lokalne mreže s oblakom.
* __Izračun__: korisnike programa Hyper-V lokalnog možete "Podignite i shift" postojeće virtualnim strojevima (VMs) za Azure.
* __Prostor za pohranu__: StorSimple se prenosi datotečnom sustavu Azure prostora za pohranu. Servis za Azure sigurnosne kopije daje sigurnosne kopije za datoteke i baze podataka SQL Azure prostora za pohranu.
* __Replikacija baze podataka__: pomoću SQL Server 2014 (ili noviji) dostupnost grupa možete implementirati visoke dostupnosti i Izrada oporavak za lokalne podatke.

##<a name="networking"></a>Povezivanje s mrežom

Azure virtualne mreže možete koristiti da biste stvorili logično Izolirani sekcije u Azure i sigurno je povezati s podatkovnim centrom na lokalni ili jedan klijentskog računala pomoću IPsec vezu. S virtualne mreže, možete iskoristiti prednost infrastrukture prilagodljivi, na zahtjev u Azure tijekom omogućuje povezivanje s poslovnim podacima i aplikacijama na lokalni, uključujući na Windows Server, mainframes i UNIX operacijskim sustavima. Dodatne informacije potražite [Azure umrežavanje dokumentacije](../virtual-network/virtual-networks-overview.md) .

##<a name="compute"></a>Izračun

Ako koristite Hyper-V lokalnog, možete "Podignite i shift" postojeće virtualnim strojevima za Azure i davatelji sa sustavom Windows Server 2012 (ili noviji), bez promjena na VM ili pretvaranje VM oblikuje usluge. Dodatne informacije potražite u članku [o i VHDs za Azure virtualnih računala](../virtual-machines/virtual-machines-linux-about-disks-vhds.md).

##<a name="azure-site-recovery"></a>Oporavak Azure web-mjesta

Ako želite Izrada oporavak kao service (DRaaS), Azure omogućuje [Oporavak Azure web-mjesta](https://azure.microsoft.com/services/site-recovery/). Oporavak Azure web-mjesta štiti potpun VMware, Hyper-V i fizičke poslužiteljima. Uz Azure oporavak web-mjesta, možete koristiti neki drugi lokalnog poslužitelja ili Azure kao web-mjesto za oporavak. Dodatne informacije o oporavak Azure web-mjesta potražite u [dokumentaciji za oporavak Azure web-mjesta](https://azure.microsoft.com/documentation/services/site-recovery/).

##<a name="storage"></a>Prostor za pohranu

Postoji nekoliko mogućnosti za korištenje Azure kao sigurnosnu kopiju web-mjesto za lokalne podatke.

###<a name="storsimple"></a>StorSimple

StorSimple sigurno i proziran integrira za pohranu u oblaku za lokalni aplikacije. Nudi jedan uređaj koji nudi visokih performansi tiered lokalne i za pohranu u oblaku, uživo arhiviranje, zaštitu oblaku podataka i Izrada oporavak. Dodatne informacije potražite u članku [StorSimple proizvoda stranice](https://azure.microsoft.com/services/storsimple/).

###<a name="azure-backup"></a>Azure sigurnosnog kopiranja

Azure sigurnosne kopije omogućuje sigurnosne kopije u oblak pomoću poznatih alata sigurnosne kopije u sustavu Windows Server 2012 (ili noviji), Windows Server 2012 Essentials (ili noviji) i sustava centra 2012 Upravitelj zaštite podataka (ili noviji). Ti alati omogućuju tijeka rada za sigurnosne kopije upravljanje koji ne ovisi mjesta za pohranu sigurnosne kopije, bez obzira na lokalni disk ili Azure prostora za pohranu. Kada je u oblak sigurnosnu podataka, ovlašteni korisnici jednostavno možete oporaviti sigurnosne kopije na bilo kojem poslužitelju.

S rastuće sigurnosne kopije, promjena na datotekama prenose se u oblak. To omogućuje učinkovito korištenje prostora za pohranu, smanjiti potrošnju propusnosti i podržava točke u vrijeme oporavak više verzija podataka. Možete odabrati i dodatne značajke, kao što su pravila zadržavanja podataka, sažimanje podataka i ograničavanje prijenos podataka. Korištenje Azure kao mjesto za sigurnosne kopije ima prednost očite li sigurnosnih kopija se automatski "teren". Time se uklanja dodatni preduvjeti za zaštita on-site medij.

Dodatne informacije potražite u članku [što je sigurnosne kopije Azure?](../backup/backup-introduction-to-azure-backup.md) i [Konfiguriranje Azure sigurnosne kopije podataka DPM](https://technet.microsoft.com/library/jj728752.aspx).

##<a name="database"></a>Baze podataka

Možete imati rješenja za oporavak Izrada baze podataka sustava SQL Server u okruženju hibridnog IT pomoću grupe dostupnosti AlwaysOn, baze podataka, zapisnika dostavu i sigurnosno kopiranje i vraćanje s spremište blobova platforme Azure. Sve ova rješenja pomoću SQL Server pokrenut na virtualnim strojevima sa sustavom Azure.

Grupe dostupnosti AlwaysOn može se koristiti u okruženju hibridnog IT gdje postoji replike baze podataka i lokalne i u oblaku. To je prikazano na sljedećem su dijagramu.

![SQL Server AlwaysOn dostupnost grupe u oblak arhitekturu hibridnog](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-3.png)

Baze podataka mogu obuhvaćati lokalne poslužitelje i oblaka u instalacijski program utemeljen na certifikata. Sljedeći dijagram prikazuje tog koncepta.

![SQL Server baze podataka sustava u oblaku arhitekturu hibridnog](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-4.png)

Zapisnik dostave mogu se sinkronizirati bazu podataka na lokalni s bazom podataka za SQL Server Azure virtualnog računala.

![SQL Server zapisnika uključena u oblak arhitekturu hibridnog](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-5.png)

Na kraju, možete stvoriti sigurnosnu baze podataka na lokalni izravno u spremište blobova platforme Azure.

![Sigurnosno kopiranje sustava SQL Server u spremište blobova platforme Azure u oblak arhitekturu hibridnog](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-6.png)

Dodatne informacije potražite u članku [visoke dostupnosti i Izrada oporavak za SQL Server na virtualnim računalima za Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md) i [sigurnosno kopiranje i vraćanje za SQL Server na virtualnim računalima za Azure](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).

##<a name="checklists-for-on-premises-recovery-in-microsoft-azure"></a>Kontrolni popisi za oporavka lokalnog sustava Microsoft Azure

###<a name="networking"></a>Povezivanje s mrežom

  1. Pročitajte odjeljak povezivanje s mrežom ovog dokumenta.
  2. Korištenje virtualne mreže za sigurno povezivanje lokalnih u oblak.

###<a name="compute"></a>Izračun

  1. Pročitajte odjeljak računalnim ovog dokumenta.
  2. Premještaju VMs između Hyper-V i Azure.

###<a name="storage"></a>Prostor za pohranu

  1. Pročitajte odjeljak za pohranu ovog dokumenta.
  2. Iskoristite prednost StorSimple usluge za korištenje za pohranu u oblaku.
  3. Pomoću servisa Azure sigurnosnu kopiju.

###<a name="database"></a>Baze podataka

  1. Pročitajte odjeljak baze podataka ovog dokumenta.
  2. Razmislite o korištenju sustava SQL Server na Azure VMs kao sigurnosnu kopiju.
  3. Postavljanje grupe dostupnosti AlwaysOn.
  4. Konfiguriranje certifikata sustavom baze podataka.
  5. Korištenje zapisnika dostave.
  6. Da biste spremište blobova platforme Azure sigurnosnu kopiju lokalne baze podataka.

##<a name="next-steps"></a>Daljnji koraci

Ovaj je članak dio niza filtriran na [Azure otpornost Tehnički vodič](./resiliency-technical-guidance.md). Sljedeći članak u ovoj seriji je [oporavak od oštećenja podataka ili nenamjerno brisanje](./resiliency-technical-guidance-recovery-data-corruption.md).