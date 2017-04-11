<properties
    pageTitle="Replikacija Hyper-V uz oporavak web-mjesta za Azure | Microsoft Azure"
    description="U ovom se članku koristite da biste shvatili Tehnički pojmovi koje olakšavaju uspješno instalirati, konfigurirati i upravljati oporavak Azure web-mjesta."
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="mkjain"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/12/2016"
    ms.author="rajanaki"/>  


# <a name="hyper-v-replication-with-azure-site-recovery"></a>Replikacija Hyper-V uz oporavak Azure web-mjesta

U ovom se članku opisuju Tehnički pojmovi koje omogućuju uspješno upravljanje i konfiguriranje Hyper-V web-mjesta ili web-mjestu sustava centra virtualnog računala Manager (VMM) Azure Zaštita pomoću oporavak Azure web-mjesta.

## <a name="setting-up-the-source-environment-for-replication-between-on-premises-and-azure"></a>Postavljanje izvora okruženje za replikaciju između lokalnog i Azure

Kao dio postavljanja Izrada oporavak između lokalnog i Azure, obavezno preuzmite i instalirajte davatelja za oporavak Azure web-mjesta na poslužitelju VMM. Instalirajte agenta servisa za oporavak Azure na svakom računalu koje hostira Hyper-V.

![VMM implementaciju web-mjesta za replikaciju između lokalnog i Azure](media/site-recovery-understanding-site-to-azure-protection/image00.png)

Postavljanje okruženja izvora u Hyper-V upravlja Infrastruktura je slična da biste postavili okruženje izvora u VMM infrastruktura za upravljane. Jedina razlika je su instalirani na računalu koje hostira Hyper-V sam davatelja i agent. U okruženju VMM davatelj je instalirana na poslužitelju VMM i agenta je instaliran na domaćini Hyper-V (u slučaju replikacije za Azure).

## <a name="workflows"></a>Tijekovi rada

### <a name="enable-protection"></a>Omogući zaštitu
Nakon što s portala za Azure i lokalnih zaštitite virtualnog računala, pokreće se zadatka Oporavak web-mjesta pod nazivom **Omogući zaštitu** . Moguće je nadzirati na kartici **Zadaci** .

!["Omogući zaštitu" zadatak na popisu zadataka](media/site-recovery-understanding-site-to-azure-protection/image001.PNG)

**Omogući zaštitu** posao provjerava preduvjete prije pozivanje metodu [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) . Ta metoda stvara replikacije za Azure pomoću unosa konfiguriranih tijekom zaštitu.

Zadatak **Omogući zaštitu** pokreće početne replikacije lokalnim po pozivanje metodu [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) . Ta metoda šalje virtualnog računala virtualne diskova Azure.

![Detalje o posao "Omogući zaštitu"](media/site-recovery-understanding-site-to-azure-protection/IMAGE002.PNG)

### <a name="finalize-protection-on-the-virtual-machine"></a>Dovršavanje zaštite virtualnog računala
[Snimka Hyper-V VM](https://technet.microsoft.com/library/dd560637.aspx) koristi se kada se pokrene početne replikacije. Virtualni tvrdi disk su obrađeni jednu po jednu dok sve diskova prenose Azure. To obično potrebno neko vrijeme da biste završili, ovisno o veličini diska i propusnosti. Da biste optimizirali Upotreba mreže, potražite [u](https://support.microsoft.com/kb/3056159)članku Upravljanje lokalnim da biste korištenja propusnosti mreže Azure zaštitu.

Po završetku početne replikacije posao **Finalize zaštite virtualnog računala** konfigurira postavke mreže i nakon replikacije. Dok je početna replikacije u tijeku:

- Evidentiraju se sve promjene u diskova. 
- Dodatni diskovni prostor za pohranu potrošnje snimke i datoteka zapisnika Hyper-V replike (HRL).

Po dovršetku početne replikacije, briše se snimka VM Hyper-V. Brisanja rezultat spajanja promjene podataka nakon početnog replikacije nadređeni disk.

!["Dovršavanje zaštite virtualnog računala" zadatak na popisu zadataka](media/site-recovery-understanding-site-to-azure-protection/image03.png)

### <a name="delta-replication"></a>Delta replikacije
Hyper-V replike replikacije evidencija, koja je dio značajke Hyper-V replike replikacije modul, evidentirati promjene virtualne tvrdi disk kao datoteke zapisnika replike Hyper-V (*.hrl). Datoteke HRL su u direktoriju isti kao pridruženi diskova.

Svaki disk koji je konfiguriran za replikaciju imaju pridruženu datoteku HRL. Ovaj zapisnik šalje se s računom za pohranu klijenta nakon početnog replikacije. Kada je zapisnik na putu do Azure, promjene u primarni se evidentiraju na drugu datoteku zapisnika u istoj mapi.

Tijekom početne replikacije ili delta replikacije, možete nadzirati VM replikacije stanja u prikazu VM, kao što je rečeno [nadzor replikacije stanja za virtualnog računala](./site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machine).  

### <a name="resynchronization"></a>Resynchronization
Virtualnog računala označen je za resynchronization kada ne uspije delta replikacije i potpuni početne replikacije je skup pomoću propusnost mreže ili vremena. Ako, na primjer, kada je veličina datoteke HRL skupine do 50 posto veličinu ukupni diska, virtualnog računala označen je za resynchronization. Resynchronization minimizira količinu podataka koji se šalju putem mreže računalstvo checksums virtualnog računala diskova izvorišno i odredišno i slanje samo u sigurnosnoj kopiji promjena.

Po završetku resynchronization normalni delta replikacije trebali biste nastavili. Možete nastaviti resynchronization ako dođe do prekida mreže ili neki drugi prekida.

Prema zadanim postavkama automatski zakazan resynchronization konfiguriran da se dogodi izvan radnog vremena. Ako je potrebno ručno resynchronized virtualnog računala, odaberite virtualnog računala s portala sustava, a zatim kliknite **ponovno sinkroniziranje**.

![Ručno resynchronization](media/site-recovery-understanding-site-to-azure-protection/image04.png)

Resynchronization koristi algoritam chunking-bloka podjele izvorišno i odredišno datoteke u fixed blokova. Checksums za svaki blok se generiraju i usporedbi da biste utvrdili koja onemogućava iz izvorišnog web-mjesta morate zatvoriti cilj.

### <a name="retry-logic"></a>Ponovnog pokušaja
Postoji logike ugrađene pokušaj replikacije pogrešaka. U ovom logike mogu podijeliti u dvije kategorije:

| Kategorija                  | Scenariji                                    |
|---------------------------|----------------------------------------------|
| Osobe koje nisu oporaviti pogreške     | Nema pokušaj pokušava. Status replikacije virtualnog računala nije **od ključne važnosti**, a potreban je administrator intervencije. Primjeri: <ul><li>Prekida VHD lanac</li><li>Stanje nije valjano za replike virtualnog računala</li><li>Pogreška prilikom provjere autentičnosti za mrežu</li><li>Pogreška autorizacije</li><li>Virtualnog računala koja nije pronađena, u slučaju samostalni Hyper-V poslužitelj</li></ul>|
| Popravljiva pogreška         | Ponovne pokušaje pojaviti svakom intervalu replikacije pomoću programa eksponencijalne natrag slobodno koja se povećava interval pokušaj od početka Prvi pokušaj (1, 2, 4, 8, 10 minuta). Ako se pogreška nastavi pojavljivati, pokušajte ponovno svakih 30 minuta. Primjeri: <ul><li>Mrežna pogreška</li><li>Malo prostora na disku</li><li>Uvjet nedovoljno memorije</li></ul>|

## <a name="hyper-v-virtual-machine-protection-and-recovery-life-cycle"></a>Hyper-V virtualnog računala zaštitu i oporavak životnog ciklusa

![Hyper-V virtualnog računala zaštitu i oporavak životnog ciklusa](media/site-recovery-understanding-site-to-azure-protection/image05.png)

## <a name="other-references"></a>Ostale reference

- [Praćenje i rješavanje problema s zaštitu VMware, VMM, Hyper-V i fizičkih web-mjesta](./site-recovery-monitoring-and-troubleshooting.md)
- [Obraćate za Microsoft Support](./site-recovery-monitoring-and-troubleshooting.md#reaching-out-for-microsoft-support)
- [Uobičajene pogreške oporavak Azure web-mjesta i njihove razlučivosti](./site-recovery-monitoring-and-troubleshooting.md#common-asr-errors-and-their-resolutions)
