<properties
    pageTitle="Azure matrice podrška za oporavak web-mjesta | Microsoft Azure"
    description="Navedene su podržani operacijski sustavi i komponente za oporavak Azure web-mjesta"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="azure-site-recovery-support-matrix"></a>Azure matrice podrška za oporavak web-mjesta

U ovom se članku navedene su podržani operacijski sustavi i komponente za oporavak web-mjesta Azure implementacije. Popis podržanih komponente i preduvjeti dostupna je za svaki scenarij implementacije u svakoj odgovarajući članak implementacije i ovaj dokument radi njihov sažetak.

## <a name="supported-operating-systems-for-virtualization-servers"></a>Podržani operacijski sustavi za poslužitelje virtualizacije


**Izvor** | **Cilj** | **Glavno računalo s operacijskim Sustavom**
---|---|---|--- 
**Glavno računalo Hyper-V (bez VMM)** | Azure | Windows Server 2012 R2 s najnovijim ažuriranjima
**Glavno računalo Hyper-V (s VMM)** | Azure | Windows Server 2012 R2 s najnovijim ažuriranjima
**Glavno računalo Hyper-V (s VMM)** | Sekundarni VMM web-mjesta | At najmanje Windows Server 2012 s najnovijim ažuriranjima
**Glavno računalo vCenter VMware** | Azure | vCenter 5,5 ili 6.0 (podrška za 5,5 značajke) <br/><br/> vSphere 6.0, 5,5 ili 5.1 s najnovijim ažuriranjima
**Glavno računalo vCenter VMware** | Sekundarni VMware web-mjesta | vCenter 5,5 ili 6.0 (podrška za 5,5 značajke) <br/><br/> vSphere 6.0, 5,5 ili 5.1 s najnovijim ažuriranjima

## <a name="supported-requirements-for-replicated-machines"></a>Podržani preduvjeti za repliciranu računalima

**Izvor** | **Što je replicirati** | **Cilj** | **Glavno računalo s operacijskim Sustavom**
---|---|---|--- 
**VMs Hyper-V** | Bilo koji radno opterećenje | Azure | Bilo koji goste OS [podržava Azure](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs mora zadovoljavati [Azure uvjete](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMs Hyper-V (s VMM)** | Bilo koji radno opterećenje | Azure | Bilo koji goste OS [podržava Azure](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs mora zadovoljavati [Azure uvjete](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMs Hyper-V (s VMM)** | Bilo koji radno opterećenje | Sekundarni VMM web-mjesta | Bilo koji goste OS [podržava Hyper-V](https://technet.microsoft.com/library/mt126277.aspx)
**VMware VMs** | Bilo koji radno opterećenje sustavom Windows VM | Azure | 64-bitni Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 sa pri najmanje SP1<br/><br/> VMs mora zadovoljavati [Azure uvjete](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMware VMs** | Bilo koji radno opterećenje sustavom Linux VM | Azure | Crvena je vaša Enterprise Linux 6,7, 7.1, 7,2 <br/><br/> Centos 6.5, 6.6, 6,7, 7.0, 7.1, 7,2 <br/><br/> Oracle Enterprise Linux 6,4, 6.5 pokrenut je vaša crveno kompatibilne otklanjanje ili Unbreakable Enterprise otklanjanje izdanje 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Spremište potrebno: File system (EXT3, ETX4, ReiserFS, XFS); Multipath softver uređaji mapiranje (multipath)); Upravitelj glasnoće: (LVM2). Fizička poslužitelji za pohranu kontroler HP CCISS nisu podržane. Datotečnom sustavu ReiserFS podržana je samo na SUSE Linux Enterprise Server 11 SP3.<br/><br/> VMs mora zadovoljavati [Azure uvjete](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMware VMs** | Bilo koji radno opterećenje sustavom Windows VM | Sekundarni VMware web-mjesta | 64-bitni Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 sa pri najmanje SP1
**VMware VMs** | Bilo koji radno opterećenje sustavom Linux VM | Sekundarni VMware web-mjesta | Crvena je vaša Enterprise Linux 6,7, 7.1, 7,2 <br/><br/> Centos 6.5, 6.6, 6,7, 7.0, 7.1, 7,2 <br/><br/> Oracle Enterprise Linux 6,4, 6.5 pokrenut je vaša crveno kompatibilne otklanjanje ili Unbreakable Enterprise otklanjanje izdanje 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Spremište potrebno: File system (EXT3, ETX4, ReiserFS, XFS); Multipath softver uređaji mapiranje (multipath)); Upravitelj glasnoće: (LVM2). Fizička poslužitelji za pohranu kontroler HP CCISS nisu podržane. Datotečnom sustavu ReiserFS podržana je samo na SUSE Linux Enterprise Server 11 SP3.
**Fizička poslužitelja** | Bilo koji radno opterećenje sustavom Windows | Azure | 64-bitni Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 sa pri najmanje SP1
**Fizička poslužitelja** | Bilo koji radno opterećenje sustavom Linux | Azure | Crvena je vaša Enterprise Linux 6.7,7.1,7.2 <br/><br/> Centos 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6,4, 6.5 pokrenut je vaša crveno kompatibilne otklanjanje ili Unbreakable Enterprise otklanjanje Release 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Spremište potrebno: File system (EXT3, ETX4, ReiserFS, XFS); Multipath softver uređaji mapiranje (multipath)); Upravitelj glasnoće: (LVM2). Fizička poslužitelji za pohranu kontroler HP CCISS nisu podržane. Datotečnom sustavu ReiserFS podržana je samo na SUSE Linux Enterprise Server 11 SP3.
**Fizička poslužitelja** | Bilo koji radno opterećenje sustavom Windows | Sekundarni web-mjesta | 64-bitni Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 sa pri najmanje SP1
**Fizička poslužitelja** | Bilo koji radno opterećenje sustavom Linux | Sekundarni web-mjesta | Crvena je vaša Enterprise Linux 6.7,7.1,7.2 <br/><br/> Centos 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6,4, 6.5 pokrenut je vaša crveno kompatibilne otklanjanje ili Unbreakable Enterprise otklanjanje izdanje 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Spremište potrebno: File system (EXT3, ETX4, ReiserFS, XFS); Multipath softver uređaji mapiranje (multipath)); Upravitelj glasnoće: (LVM2). Fizička poslužitelji za pohranu kontroler HP CCISS nisu podržane. Datotečnom sustavu ReiserFS podržana je samo na SUSE Linux Enterprise Server 11 SP3.


## <a name="provider-versions"></a>Davatelj verzije

**Ime** | **Opis** | **Najnovija verzija** | **Podrška** | **Pojedinosti**
---|---|---|---| ---
**Davatelj usluga za oporavak Azure web-mjesta** | Koordinate komunikaciju između lokalne poslužitelje i Azure/sekundarni web-mjesta <br/><br/> Instalirano lokalne poslužitelje VMM ili poslužitelje Hyper-V ako poslužitelj VMM ne postoji | 5.1.1700 (dostupno s portala) | [Najnovije značajke i rješenja](https://support.microsoft.com/kb/3155002)
**Postavljanje (VMware za Azure) za Sjedinjeno komuniciranje oporavak Azure web-mjesta** | Koordinate komunikaciju između lokalne poslužitelje VMware i Azure <br/><br/> Instalirano lokalne poslužitelje VMware | 9.3.4246.1 (dostupno s portala) | [Najnovije značajke i rješenja](https://support.microsoft.com/kb/3155002)
**Servis za mobilnost** | Replikacija koordinate između lokalne VMware poslužitelja/fizičke poslužitelje i Azure/sekundarni web-mjesta | N/d (dostupno s portala) | Instalirana na svakom VMware VM ili fizičke poslužitelju na kojem želite replicirati. **Agent za Microsoft Azure oporavak Services (OŽU)** | Replikacija koordinate između Hyper-V VMs i Azure<br/><br/> Instalirana na lokalnim poslužiteljima Hyper-V (sa ili bez VMM server) | 2.0.8689.0 (dostupno s portala) | Ovaj agent koristi oporavak Azure web-mjesta i sigurnosno kopiranje Azure). [Dodatne informacije] (https://support.microsoft.com/en-us/kb/2997692)

## <a name="next-steps"></a>Daljnji koraci

[Priprema za implementaciju](site-recovery-best-practices.md)

