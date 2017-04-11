<properties
    pageTitle="Mapiranje pohranom na servisu Azure oporavak web-mjesta za replikaciju virtualnog računala Hyper-V između lokalnog podatkovnim centrima | Microsoft Azure"
    description="Priprema mapiranje prostora za pohranu za replikaciju virtualnog računala Hyper-V između dva lokalnog podatkovnim centrima uz oporavak Azure web-mjesta."
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
    ms.date="07/06/2016"
    ms.author="raynew"/>


# <a name="prepare-storage-mapping-for-hyper-v-virtual-machine-replication-between-two-on-premises-datacenters-with-azure-site-recovery"></a>Priprema mapiranje prostora za pohranu za replikaciju virtualnog računala Hyper-V između dva lokalnog podatkovnim centrima uz oporavak Azure web-mjesta


Oporavak Azure web-mjesta doprinosa tvrtke continuity i Izrada oporavak (BCDR) strategije po orchestrating replikacije, prebacivanje i oporavak virtualnim strojevima i fizičke poslužiteljima. U ovom se članku opisuju mapiranje prostora za pohranu koji pridonosi optimalnih koristite prostora za pohranu kada koristite oporavak web-mjesta za replikaciju Hyper-V virtualnim strojevima između podatkovnim centrima dva lokalnog VMM.

Pri dnu ovog članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)objavljuju komentare ni pitanja.

## <a name="overview"></a>Pregled

Prostor za pohranu mapiranje vrijedi samo kada ste replikaciju Hyper-V virtualnih računala koje se nalaze u oblaka VMM iz primarnog podatkovnog centra sekundarne podatkovnim centrom pomoću značajke Hyper-V replike ili SAN replikacije na sljedeći način:


- **Lokalni za lokalni replikacije s replike Hyper-V)** – Postavite prostora za pohranu mapiranje po mapiranje klasifikacije prostora za pohranu na izvoru i ciljnog VMM poslužitelji za učinite sljedeće:

    - **Otkrivanje cilj prostor za pohranu replike virtualnim strojevima**– virtualnim strojevima će je replicirati na cilj prostora za pohranu (SMB zajedničko korištenje ili skupine zajedničke količine (CSVs)) koje ste odabrali.
    - **Položaj virtualnog računala replike**– mapiranje prostora za pohranu za optimalnog postavite replike virtualnim strojevima na poslužiteljima Hyper-V glavnog računala. Replike virtualnim strojevima će smjestiti na domaćini koji mogu pristupiti klasifikacija mapiranih prostora za pohranu.
    - **Prostor za pohranu mapiranja**– ako ne konfiguriranje mapiranja prostora za pohranu, virtualnim strojevima je replicirati na zadano mjesto za pohranu naveden na poslužitelju glavno računalo Hyper-V pridružene replike virtualnog računala.

- **Lokalni za replikaciju lokalnog s SAN**– postavljanje za pohranu mapiranje po mapiranje grupe polja za pohranu na izvoru i ciljnog VMM poslužitelji.
    - **Navedite skup**– određuje koje skup sekundarne prostora za pohranu prima replikacije podatke iz primarnog grupe aplikacija.
    - **Otkrivanje ciljnu pohranu grupe**– osigurava da se LUNs u grupi replikacije izvor replicirati mapirana ciljna resurse za pohranu po izboru.

## <a name="set-up-storage-classifications-for-hyper-v-replication"></a>Postavljanje klasifikacije prostora za pohranu za replikaciju Hyper-V

Kada koristite Hyper-V replike za replikaciju s oporavak web-mjesta, mapiranja između klasifikacije prostora za pohranu na poslužiteljima VMM izvorišno i odredišno ili na jednom VMM poslužitelju ako na istom poslužitelju VMM upravlja dva web-mjesta. Imajte na umu da:

- Prilikom mapiranja ispravno konfigurirano te replikacije omogućena, virtualnog računala virtualne na tvrdom disku lokaciji primarni je replicirati prostor za pohranu u mapiranih odredištu.
- Prostor za pohranu klasifikacije moraju biti dostupne grupama glavnog računala koja se nalazi u izvorišno i odredišno oblaka.
- Klasifikacije ne moraju imati istu vrstu prostora za pohranu. Ako, na primjer, možete mapirati klasifikacija izvora koji sadrži SMB zajedničko korištenje da biste ciljnu klasifikacija koja sadrži CSVs.
- Pročitajte više u [stvaranju klasifikacije prostora za pohranu u VMM](https://technet.microsoft.com/library/gg610685.aspx).

## <a name="example"></a>Primjer

Ako klasifikacije ispravno konfigurirane u VMM kad odaberete izvorišno i odredišno VMM poslužitelja tijekom mapiranja prostora za pohranu, prikazat će se klasifikacije izvorišno i odredišno. Evo primjera zajedničko korištenje datoteka za pohranu i klasifikacije za tvrtku ili ustanovu s dva mjesta u New Yorku i Chicago.

**Mjesto** | **VMM poslužitelja** | **Zajedničko korištenje datoteka (izvor)** | **Klasifikacija (izvor)** | **Mapirani** | **Zajedničko korištenje datoteka (cilj)**
---|---|--- |---|---|---
New York | VMM_Source| SourceShare1 | ZLATNA | GOLD_TARGET | TargetShare1
 |  | SourceShare2 | SREBRNO | SILVER_TARGET | TargetShare2
 | | SourceShare3 | BRONZANI | BRONZE_TARGET | TargetShare3
Chicago | VMM_Target |  | GOLD_TARGET | Nisu mapirani |
| | | SILVER_TARGET | Nisu mapirani |
 | | | BRONZE_TARGET | Nisu mapirani

Želite konfigurirati te na kartici **Poslužitelj za pohranu** u **Resursi** stranice portala za oporavak web-mjesta.

![Konfiguriranje mapiranja prostora za pohranu](./media/site-recovery-storage-mapping/storage-mapping1.png)

S ovom primjeru:
- Kada je za sve virtualnog računala na ZLATNA prostora za pohranu (SourceShare1) stvara se replike virtualnog računala, će je replicirati na pohranu GOLD_TARGET (TargetShare1).
- Replike virtualnog računala stvaranja za sve virtualnog računala na SREBRNI prostora za pohranu (SourceShare2) će je replicirati na SILVER_TARGET (TargetShare2) prostora za pohranu i tako dalje.

Zajedničko korištenje datoteke i njihove dodijeljene klasifikacije u VMM prikazuju se na sljedećem zaslonu snimka.

![Prostor za pohranu klasifikacije u VMM](./media/site-recovery-storage-mapping/storage-mapping2.png)

## <a name="multiple-storage-locations"></a>Više mjesta za pohranu

Ako klasifikacija cilj je dodijeljen više SMB dionice ili CSVs, mjesto optimalnih pohrane označit će se automatski kada je zaštićen virtualnog računala. Ako bez prostora za pohranu odgovarajuću cilj dostupan je sa navedeni klasifikacija, zadano mjesto za pohranu naveden na glavnom računalu Hyper-V se koristi da biste postavili replike virtualne tvrdom disku.

U sljedećoj su tablici prikazali kako klasifikacija prostora za pohranu i količine klaster zajednički postavljeni u našem primjeru.

**Mjesto** | **Klasifikacija** | **Pridruženi prostora za pohranu**
---|---|---
New York | ZLATNA | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p>
 | SREBRNO | <p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p>
Chicago | GOLD_TARGET | <p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p>
 | SILVER_TARGET| <p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p>

U ovoj su tablici navedene ponašanje kada omogućite zaštitu virtualnim strojevima (VM1 - VM5) u ovom primjeru okruženju.

**Virtualnog računala** | **Prostor za pohranu izvora** | **Klasifikacija izvora** | **Mapirana ciljna prostora za pohranu**
---|---|---|---
VM1 | C:\ClusterStorage\SourceVolume1 | ZLATNA | <p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Obje GOLD_TARGET</p>
VM2 | \\FileServer\SourceShare1 | ZLATNA | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Obje GOLD_TARGET</p>
VM3 | C:\ClusterStorage\SourceVolume2 | SREBRNO | <p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p>
VM4 | \FileServer\SourceShare2 | SREBRNO |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Obje SILVER_TARGET</p>
VM5 | C:\ClusterStorage\SourceVolume3 | N/D | Nema mapiranja, tako da se koristi zadano mjesto za pohranu glavnog računala Hyper-V

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste bolje razumjeli mapiranja prostora za pohranu, [Priprema za implementaciju oporavak Azure web-mjesta](site-recovery-best-practices.md).
