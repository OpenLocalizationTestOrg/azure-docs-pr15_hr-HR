<properties
    pageTitle="Priprema preslikavanje za zaštitu virtualnog računala Hyper-V s VMM u oporavak web-mjesta Azure | Microsoft Azure"
    description="Postavljanje preslikavanje za Hyper-V virtualnog računala replikaciju iz podatkovnog centra sustava lokalnog Azure ili sekundarnog web-mjesta."
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


# <a name="prepare-network-mapping-for-hyper-v-virtual-machine-protection-with-vmm-in-azure-site-recovery"></a>Priprema preslikavanje za zaštitu virtualnog računala Hyper-V s VMM u oporavak Azure web-mjesta

Oporavak Azure web-mjesta doprinosa tvrtke continuity i Izrada oporavak (BCDR) strategije po orchestrating replikacije, prebacivanje i oporavak virtualnim strojevima i fizičke poslužiteljima.

U ovom se članku opisuju preslikavanje koja olakšava optimalnog konfiguriranja postavki mreže kada koristite oporavak web-mjesta za replikaciju Hyper-V virtualnim strojevima nalazi u VMM oblaka između dva podatkovnim centrima lokalnog ili između lokalnog podatkovnog centra i Azure. Imajte na umu da ako koristite replikaciju Hyper-V VMs bez VMM oblaka ili replikaciju VMware VMs ili fizičke poslužitelji, u ovom se članku nije odgovarajući.

Pri dnu ovog članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)objavljuju komentare ni pitanja.


## <a name="overview"></a>Pregled

Preslikavanje koristi se kada oporavak web-mjesta Azure je implementiran za replikaciju Hyper-V virtualnim strojevima Azure ili sekundarni podatkovnog centra, pomoću značajke Hyper-V replike ili SAN replikacije.

- **Replikaciju Hyper-V virtualnim strojevima u VMM oblaka između dvaju lokalnog podatkovnim centrima**– mrežne mape mapiranja između VM mrežama na poslužitelju VMM izvora i VM mrežama na poslužitelju ciljni VMM, učinite sljedeće:

    - **Povezivanje virtualnim strojevima nakon prebacivanje**– osigurava da virtualnim strojevima će se povezati s odgovarajućim mrežama nakon prebacivanje. Replike virtualnog računala će biti povezani s mrežom ciljne koja je mapirana mrežu izvora.
    - **Mjesto replike virtualnim strojevima na poslužiteljima glavno računalo**– optimalnog postavite replike virtualnim strojevima na poslužiteljima Hyper-V glavnog računala. Replike virtualnim strojevima će se smjestiti na domaćini koji mogu pristupiti mapiranih VM mrežama.
    - **Nema preslikavanje**– ako ne konfigurirate preslikavanje, repliciranu virtualnim strojevima neće biti povezani s nijednu mrežu VM nakon prebacivanje.

- **Replikaciju Hyper-V virtualnim strojevima u na lokalni VMM u oblaku za Azure**– mrežne mape mapiranja između VM mreže na izvornom poslužitelju VMM i ciljnog Azure mrežama, učinite sljedeće:
    - **Povezivanje virtualnim strojevima nakon prebacivanje**– svim računalima koje prebacivanje na istoj mreži možete se povezati s drugoga, bez obzira koju tarifu za oporavak se ona nalazi u.
    - **Mrežni pristupnik**– ako pristupnika mreže postavljena na odredištu Azure mreže, virtualnim strojevima možete povezati s drugim lokalnog virtualnim računalima sustava.
    - **Nema preslikavanje**– ako ne konfigurirali preslikavanje, samo virtualnim strojevima te prebacivanje u istu tarifu oporavak moći povezati međusobno nakon prekida putem Azure.


## <a name="network-mapping-example"></a>Primjer mapiranje mreže

Preslikavanje se može konfigurirati između VM mreže na dva VMM poslužitelja ili na jednom VMM poslužitelju ako dva web-mjesta kojima upravlja na isti poslužitelj. Kada mapiranje ispravno konfigurirano te replikacije omogućena, virtualnog računala na primarnom mjestu će biti povezani s mrežom i njegov replike na ciljnom mjestu će se povezati s mapiranih mrežom.

Ako mreža su postavljene pravilno u VMM, kad odaberete mreže VM cilj tijekom preslikavanje, oblaka VMM izvora koji koriste VM mreže izvora, pojavit će se zajedno s mreže VM dostupna cilj na ciljnom oblaka koji se koriste za zaštitu.

Slijedi primjer predočiti ovaj mehanizam. Pogledajmo o tvrtki ili ustanovi s dva mjesta u New Yorku i Chicago.

**Mjesto** | **VMM poslužitelja** | **VM mreža** | **Mapirani**
---|---|---|---
New York | VMM NewYork| VMNetwork1 NewYork | Mapirani VMNetwork1 Chicago
 |  | VMNetwork2 NewYork | Nisu mapirani
Chicago | VMM Chicago| VMNetwork1 Chicago | Mapirani VMNetwork1 NewYork
 | | VMNetwork2 Chicago | Nisu mapirani

S ovom primjeru:

- Replike virtualnog računala stvaranja za virtualnog računala koja je povezana s VMNetwork1 NewYork će se povezati s VMNetwork1 Chicago.
- Replike virtualnog računala stvaranja VMNetwork2 NewYork ili VMNetwork2 Chicago ga će nije povezano s mreže.

Evo kako su VMM oblaka postaviti u našem primjeru tvrtke ili ustanove i logički mreža pridružene oblaka.

### <a name="cloud-protection-settings"></a>Postavke zaštite oblaka

**Zaštićeni oblaka** | **Zaštita oblaka** | **Logička mreže (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>N/D</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>
SilverCloud2 | <p>N/D</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>

### <a name="logical-and-vm-network-settings"></a>Logika i VM postavke mreže

**Mjesto** | **Logička mreže** | **Pridruženi VM mreže**
---|---|---
New York | LogicalNetwork1 NewYork | VMNetwork1 NewYork
Chicago | LogicalNetwork1 Chicago | VMNetwork1 Chicago
 | LogicalNetwork2Chicago | VMNetwork2 Chicago

### <a name="target-networks"></a>Cilj mreža

Kad odaberete VM mreže cilj, ovisno o tim postavkama, Sljedeća tablica prikazuje mogućnosti koje će biti dostupne.

**Odaberite** | **Zaštićeni oblaka** | **Zaštita oblaka** | **Dostupne mreže cilj**
---|---|---|---
VMNetwork1 Chicago | SilverCloud1 | SilverCloud2 | Dostupna
 | GoldCloud1 | GoldCloud2 | Dostupna
VMNetwork2 Chicago | SilverCloud1 | SilverCloud2 | Nije dostupno
 | GoldCloud1 | GoldCloud2 | Dostupna



## <a name="multiple-subnets"></a>Više podmreže

Ako mreža cilj ima više podmreže i jednu od tih podmreže ima isti naziv kao podmreže na kojem se nalazi virtualnog računala izvora, zatim virtualnog računala replike će se povezati s tom podmreže ciljne nakon prebacivanje. Ako nema cilj podmreže pod nazivom koji se podudaraju, virtualnog računala će se povezati s prvom podmreže u mreži.


### <a name="failback"></a>Failback

Da biste vidjeli što se događa ako failback (obrnutim replikacije), recimo pretpostavlja da je VMNetwork1 NewYork na VMNetwork1 Chicago mapirana s sljedeće postavke.


**Virtualnog računala** | **Povezani s mrežom VM**
---|---
VM1 | VMNetwork1 mreže
VM2 (kopije VM1) | VMNetwork1 Chicago

Pomoću ove postavke Pogledajmo što se događa u nekoliko mogućih scenarijima.

**Scenarij** | **Rezultat**
---|---
Nema promjene u svojstva mreže VM 2 nakon prebacivanje. | VM 1 ostati povezani s mrežom izvora.
Svojstva mreže VM 2 mijenjaju nakon prebacivanje i isključen. | Veza je prekinuta VM 1.
Svojstva mreže VM 2 mijenjaju nakon prebacivanje i povezano s VMNetwork2 Chicago. | Ako nije mapirana VMNetwork2 Chicago VM 1 će prekinuta.
Mapiranje mreže VMNetwork1 Chicago se mijenja. | VM 1 će biti povezani s mrežom sada mapirati VMNetwork1 Chicago.


## <a name="next-steps"></a>Daljnji koraci

Sad kad ste bolje razumjeli preslikavanje [Početak rada s implementacije oporavak web-mjesta](site-recovery-best-practices.md).
