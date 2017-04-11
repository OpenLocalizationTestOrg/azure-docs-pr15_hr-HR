<properties
    pageTitle="Test performanse i omjere rezultate za lokalni u lokalni Hyper-V replikacije s web-mjesta oporavak | Microsoft Azure"
    description="Ovaj članak sadrži informacije o performansama testiranje za lokalni za lokalni replikacije pomoću oporavak Azure web-mjesta."
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
    ms.date="07/06/2016"
    ms.author="raynew"/>

# <a name="performance-test-and-scale-results-for-on-premises-to-on-premises-hyper-v-replication-with-site-recovery"></a>Performanse testirajte i skaliranje rezultata za lokalni za replikaciju Hyper-V lokalnog s oporavak web-mjesta

Oporavak web-mjesta za Microsoft Azure možete koristiti za orkestrirali i upravljanje replikacije virtualnim strojevima i fizičke poslužitelje Azure ili sekundarni podatkovnog centra. U ovom se članku daje rezultate performanse testiranja smo jeste li kada replikaciju Hyper-V virtualnim strojevima između dvaju lokalnog podatkovnim centrima.



## <a name="overview"></a>Pregled

Testiranje cilj je da biste pregledali kako oporavak web-mjesta Azure izvodi tijekom replikacije steady stanje. Stanje steady replikacije pojavljuje se kada virtualnim strojevima dovršite početne replikacije i sinkronizirate delta promjene. Važno je da mjerenje performansama pomoću steady stanje jer je stanje u kojem Većina virtualnim strojevima ostaju osim ako neočekivane kvarove će se pojaviti.


Uvođenje test consisted dva lokalnog web-mjesta s poslužiteljem VMM u svakom web-mjestu. Ovaj test implementacije je uobičajeni glavni office/granu implementacija sustava office, s glavni office ulozi primarni web-mjesta i podružnici kao sekundarnog ili oporavak web-mjesta.

### <a name="what-we-did"></a>Ne možemo radnje

Evo što smo jeste li u testu proći:

1. Stvara virtualnim strojevima pomoću VMM predložaka.

1. Prvi koraci virtualnim strojevima i snimanje referentne vrijednosti performansi metriku veće od 12 sati.

1. Stvoreno oblaka na primarni i oporavak VMM poslužiteljima.

1. Zaštita konfigurirani oblak u Azure oporavak web-mjesta, uključujući mapiranje izvora i oporavak oblaka.

1. Omogućuje zaštitu virtualnim strojevima i dopustite potpuni početne replikacije.

1. Waited nekoliko sati za stabilization sustava.

1. Veće od 12 sati, zabilježene performanse metriku jamči da sve virtualnim strojevima ostaje u stanju očekivani replikacije te 12 sati.

1. Izmjerite delta između referentne vrijednosti performansi metriku i performanse mjerenja replikaciju.

## <a name="test-deployment-results"></a>Rezultate testiranja implementacije

### <a name="primary-server-performance"></a>Performanse primarnog poslužitelja

- Hyper-V replike asinkrono prati promjene datoteka zapisnika s indirektni minimalne prostora za pohranu na primarnom poslužitelju.

- Hyper-V replike koristi samostalno održavaju predmemoriji da biste minimizirali IOPS indirektni za praćenje. Sprema piše VHDX u memoriji i čišćenja ih u datoteci zapisnika prije vrijeme slanja Prijava na web-mjesto za oporavak. Na disku čišćenja također se događa ako na zapisivanje kliknite unaprijed određenim ograničenje.

- Grafikon sustava prikazuje stanje steady indirektnog IOPS za replikaciju. Ne možemo možete vidjeti je li IOPS indirektni zbog replikacije oko 5% koja je prilično niske.

![Primarni rezultata](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744913.png)

Hyper-V replike koristi memorije primarni poslužitelj za optimizaciju performansi diska. Kao što je prikazano u sljedećim grafikonu memorije indirektni na svim poslužiteljima u primarni klaster je granične. Memorije indirektni prikazano je postotak memorije koristi za replikaciju u usporedbi s ukupni instalirane memorije na poslužitelju Hyper-V.

![Primarni rezultata](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744914.png)

Hyper-V replike ima minimalne indirektni procesora. Kao što je prikazano u grafikonu, indirektni replikacije je u rasponu od 2-3%.

![Primarni rezultata](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744915.png)

### <a name="secondary-recovery-server-performance"></a>Performanse poslužitelja sekundarni (oporavak)

Hyper-V replike koristi mali memorije na poslužitelju za oporavak da biste optimizirali broj operacija prostora za pohranu. Na grafikonu navedene memorije na poslužitelju za oporavak. Memorije indirektni prikazano je postotak memorije koristi za replikaciju u usporedbi s ukupni instalirane memorije na poslužitelju Hyper-V.

![Sekundarni rezultata](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744916.png)

Iznos/i postupci za oporavak web-mjesta je funkcija broja operacije pisanja primarni web-mjesta. Recimo pogledajte ukupnu/i operacije na web-mjestu oporavak usporedbi ukupni/i operacije i pisanje operacije primarni web-mjesta. Prikaz grafikona prikazuju zbroj IOPS na web-mjestu oporavak je

- Oko 1,5 puta pisanja IOPS na primarni.

- Oko 37% zbroja IOPS primarni web-mjesta.

![Sekundarni rezultata](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744917.png)

![Sekundarni rezultata](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744918.png)

### <a name="effect-of-replication-on-network-utilization"></a>Učinak replikacije na Upotreba mreže

Prosjek 275 MB sekundi propusnost mreže korišten između čvorove primarni i oporavak (pomoću spajanja je omogućeno) na temelju postojeće propusnosti 5 GB u sekundi.

![Upotreba mreže rezultata](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744919.png)

### <a name="effect-of-replication-on-virtual-machine-performance"></a>Učinak replikacije na performanse virtualnog računala

Važna činjenica je učinak na replikacije na radnih opterećenja radnog izvode na virtualnim računalima. Ako na web-mjestu primarni odgovarajući način dodjeli za replikaciju, problem bi trebalo biti utjecaja na na radnih opterećenja. Hyper-V replike lightweight praćenja mehanizam osigurava da radnih opterećenja izvodi na virtualnim računalima sustava neće utjecati tijekom replikacije steady stanja. To je podržali sljedeće grafikona.

U ovom se grafikonu prikazuju IOPS izvršiti virtualnim strojevima koji se izvode drugi radnih opterećenja prije i nakon omogućivanja replikacije. Primijetit ćete da nema razlike između dva.

![Rezultati replike efekta](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744920.png)

Sljedeće grafikonu prikazuju propusnost virtualnih računala koji se izvode drugi radnih opterećenja prije i nakon omogućivanja replikacije. Primijetit ćete da replikacija utječe bez značajan.

![Efekti replike rezultata](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744921.png)

### <a name="conclusion"></a>Zaključak

Rezultati jasno prikažite mijenja li veličinu Azure oporavak web-mjesta, povezano s replike Hyper-V s najmanje indirektni za velike klaster.  Azure oporavak web-mjesta omogućuje jednostavno implementaciju, replikacije, upravljanje i nadzor. Hyper-V replike sadrži potrebne infrastrukturu za uspješan replikacije skaliranje. Za planiranje implementacije sustava najbolju predlažemo da preuzmete [Hyper-V replike kapaciteta planer](https://www.microsoft.com/download/details.aspx?id=39057).

## <a name="test-environment-details"></a>Testiranje okruženje pojedinosti

### <a name="primary-site"></a>Primarni web-mjesta

- Primarni web-mjesta sadrži klaster koja sadrži pet Hyper-V poslužitelje s programom 470 virtualnih računala.

- Virtualnim strojevima pokrenite različitih radnih opterećenja i imaju zaštitu oporavak Azure web-mjesta.

- Prostor za pohranu za čvor klaster osigurava iSCSI SAN. Model – Hitachi HUS130.

- Svaki poslužitelj klaster ima četiri mrežne kartice (NIC-ovi) od jednog Gbps.

- Dvije kartice mrežnog povezani s iSCSI privatne mreže i dva su povezani s mrežom vanjskih enterprise. Jedno od vanjskih mreža je rezervirana za komunikaciju klaster samo.

![Primarni hardverski preduvjeti](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744922.png)

|Poslužitelj|RAM-A|Model|Procesor|Broj procesora|NIC|Softver|
|---|---|---|---|---|---|---|
|Hyper-V poslužitelji u skupini: <br />ESTLAB HOST11<br />ESTLAB HOST12<br />ESTLAB HOST13<br />ESTLAB HOST14<br />ESTLAB HOST25|128ESTLAB HOST25 sadrži 256|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) CPU E5-4620 0 @ 2.20GHz|4|Li Gbps x 4|Podatkovnog centra paketa Windows Server 2012 R2 (x64) + uloga Hyper-V|
|VMM poslužitelja|2|||2|1 Gbps|Windows Server bazu podataka 2012 R2 (x 64) + VMM 2012 R2|

### <a name="secondary-recovery-site"></a>Sekundarni (oporavak) web-mjesta

- Sekundarni web-mjesta sadrži šest čvor prebacivanje klaster.

- Prostor za pohranu za čvor klaster osigurava iSCSI SAN. Model – Hitachi HUS130.

![Specifikacija primarni hardvera](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744923.png)

|Poslužitelj|RAM-A|Model|Procesor|Broj procesora|NIC|Softver|
|---|---|---|---|---|---|---|
|Hyper-V poslužitelji u skupini: <br />ESTLAB HOST07<br />ESTLAB HOST08<br />ESTLAB HOST09<br />ESTLAB HOST10|96|Dell™ PowerEdge™ R720|Intel(R) Xeon(R) CPU E5-2630 0 @ 2.30GHz|2|Li Gbps x 4|Podatkovnog centra paketa Windows Server 2012 R2 (x64) + uloga Hyper-V|
|ESTLAB HOST17|128|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) CPU E5-4620 0 @ 2.20GHz|4||Podatkovnog centra paketa Windows Server 2012 R2 (x64) + uloga Hyper-V|
|ESTLAB HOST24|256|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) CPU E5-4620 0 @ 2.20GHz|2||Podatkovnog centra paketa Windows Server 2012 R2 (x64) + uloga Hyper-V|
|VMM poslužitelja|2|||2|1 Gbps|Windows Server bazu podataka 2012 R2 (x 64) + VMM 2012 R2|

### <a name="server-workloads"></a>Radnih opterećenja poslužitelja

- U svrhu test smo izdvojiti radnih opterećenja obično se koristi u enterprise scenarija.

- Koristimo [IOMeter](http://www.iometer.org) s karakteristikama radno opterećenje navedene u tablici za simulacije.

- Svi profili IOMeter postavljene su na napišite slučajnih bajtova u programu Publisher worst-case pisanje uzoraka za radnih opterećenja.

|Radno opterećenje|/ I veličina (KB)|% Programa access|% Čitanje|Preostala I/Os|Uzorak/i|
|---|---|---|---|---|---|
|Poslužitelj datoteka|48163264|60 20 %5 %5% 10%|80% 80% 80% 80% 80%|88888|Sve 100% izravnim|
|SQL Server (jedinice 1) SQL Server (jedinice 2)|864|100% 100%|70 %% 0|88|100% random100% nizu|
|Exchange|32|100%|od 67%|8|100% izravnim|
|Radne stanice/VDI-JA|464|66% od 34%|95% 70%|11|Oba slučajni 100%|
|Web-poslužitelj datoteka|4864|33 34% 33%|95 95% 95%|888|Sve 75% izravnim|

### <a name="virtual-machine-configuration"></a>Konfiguracija virtualnog računala

- 470 virtualnim strojevima na primarni klaster.

- Sve virtualnim strojevima VHDX disku.

- Virtualnim strojevima izvodi radnih opterećenja navedene u tablici. Sve su stvorene pomoću VMM predložaka.

|Radno opterećenje|# VMs|Minimalna RAM-a (GB)|Maksimalna RAM-a (GB)|Veličina logičke diska (GB) po VM|Maksimalna IOPS|
|---|---|---|---|---|---|
|SQL Server|51|1|4|167|10|
|Exchange Server|71|1|4|552|10|
|Poslužitelj datoteka|50|1|2|552|22|
|VDI-JA|149|.5|1|80|6|
|Web-poslužitelj|149|.5|1|80|6|
|UKUPNI ZBROJ|470|||96.83 TB|4108|

### <a name="azure-site-recovery-settings"></a>Postavke Azure oporavak web-mjesta

- Informacije o lokalnom zaštita lokalnih koji je konfiguriran Azure oporavak web-mjesta

- Poslužitelj VMM ima četiri oblaka konfiguriran, koja sadrži klaster poslužitelje Hyper-V i njihovih virtualnim računalima.

|Primarni VMM oblaka|Zaštićeni virtualnih računala u oblaku|Učestalost ponavljanja|Oporavak dodatne točke|
|---|---|---|---|
|PrimaryCloudRpo15m|142|15 minuta|Ništa|
|PrimaryCloudRpo30s|47|30 sekundi|Ništa|
|PrimaryCloudRpo30sArp1|47|30 sekundi|1|
|PrimaryCloudRpo5m|235|5 minuta|Ništa|

### <a name="performance-metrics"></a>Performanse mjerenja

U tablici navedene metriku performanse i mjerača koji su mjeri implementacije.

|Mjerenja|Brojač|
|---|---|
|PROCESOR|\Processor(_Total)\% procesor vremena|
|Dostupnom memorijom|\Memory\Available MBytes|
|IOPS|Prijenosi/sec \Disk \PhysicalDisk (_Total)|
|VM čitati operacije sec (IOPS)|\Hyper-V virtualne uređaj za pohranu (<VHD>) \Read operacije/Sec|
|Operacije/sec VM pisanje (IOPS)|\Hyper-V virtualne uređaj za pohranu (<VHD>) operacije S \Write|
|VM čitati propusnost|\Hyper-V virtualne uređaj za pohranu (<VHD>) \Read bajtova/sec|
|Propusnost VM pisanja|\Hyper-V virtualne uređaj za pohranu (<VHD>) \Write bajtova/sec|


## <a name="next-steps"></a>Daljnji koraci

- [Postavljanje protection između dva lokalnog VMM web-mjesta](site-recovery-vmm-to-vmm.md)
