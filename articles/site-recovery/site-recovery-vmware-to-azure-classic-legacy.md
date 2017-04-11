<properties
    pageTitle="Replicirati VMware virtualnim strojevima i fizičke poslužitelje Azure s oporavak Azure web-mjesta (naslijeđeno) | Microsoft Azure" 
    description="Opisuje kako replicirati lokalnog VMs i Windows/Linux fizičke poslužitelji za Azure korištenjem oporavak Azure web-mjesta u naslijeđene implementacije klasični portalu." 
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
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery-using-the-classic-portal-legacy"></a>Replicirati VMware virtualnim strojevima i fizičke poslužitelje Azure s oporavak web-mjesta za Azure pomoću portala za klasični (naslijeđeno)

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-vmware-to-azure.md)
- [Klasični Portal](site-recovery-vmware-to-azure-classic.md)
- [Klasični Portal (naslijeđeno)](site-recovery-vmware-to-azure-classic-legacy.md)


Dobro došli u oporavak Azure web-mjesta! U ovom se članku opisuju naslijeđene uvođenje za replikaciju lokalnog VMware virtualnim strojevima ili Windows/Linux fizičke poslužitelje Azure pomoću oporavak Azure web-mjesta na portalu klasični.

## <a name="overview"></a>Pregled

Tvrtke i ustanove moraju strategije BCDR koja određuje kako aplikacija, radnih opterećenja i podataka ostati pokrenut i dostupne tijekom planirane i Neplanirana isključiti, a čim oporaviti normalno funkcionira uvjeta. Strategije BCDR trebali biste zadržali poslovnih podataka sigurnih i koje se mogu vratiti i provjerite je li radnih opterećenja ostaju stalno dostupne kada se pojavi Izrada.

Oporavak web-mjesta je Azure service doprinosa strategije BCDR po orchestrating replikacije lokalne poslužitelje fizičke i virtualnih računala s oblakom (Azure) ili sekundarni podatkovnog centra. Prilikom kvarove u svom primarnom mjestu, koji se neće putem sekundarne mjesto da bi aplikacija i radnih opterećenja dostupna. Ne natrag na svom primarnom mjestu kada on se vraća na običnom operacije. Dodatne informacije u [oporavak web-mjesta Azure?](site-recovery-overview.md)


>[AZURE.WARNING] Ovaj članak sadrži **upute za naslijeđene**. Nemojte koristiti ga za nove implementacije. Umjesto toga, [slijedite ove upute](site-recovery-vmware-to-azure.md) za implementaciju oporavak web-mjesta u Azure portala i [koristite ove upute](site-recovery-vmware-to-azure-classic.md) za konfiguriranje poboljšane implementacije klasični portalu. Ako ste već implementiran na način opisan u ovom članku, preporučujemo da migrirati uz Poboljšana implementaciju klasični portalu.


## <a name="migrate-to-the-enhanced-deployment"></a>Migriranje poboljšane implementacije

U ovom se odjeljku vrijedi samo ako ste već implementiran oporavak web-mjesta pomoću upute u ovom članku.

Migracija postojeće uvođenja morat ćete:

1. Implementacija nove komponente oporavak web-mjesta s lokalnim web-mjesta.
2. Konfiguriranje vjerodajnica za automatsko otkrivanje VMware VMs na novi poslužitelj za konfiguraciju.
3. Otkrijte VMware poslužiteljima sa novi poslužitelj za konfiguraciju.
3. Stvaranje nove grupe zaštitu s novi poslužitelj za konfiguraciju.


Prije nego što započnete:

- Preporučujemo da postavljanje prozora Održavanje za migraciju.
- **Migriranje strojeva** je mogućnost dostupna samo ako imate postojeću zaštitu grupe koje su stvorene tijekom naslijeđene implementacije.
- Kada dovršite korake Migracija može potrajati 15 minuta ili dulje da biste osvježili vjerodajnice, a da biste otkrili i osvježavanje virtualnim strojevima tako da ih možete dodati u grupu zaštitu. Možete osvježiti ručno umjesto na čekanju. 

Migracija na sljedeći način:

1. Saznajte više o [poboljšane implementacije klasični portalu](site-recovery-vmware-to-azure-classic.md#enhanced-deployment). Pregledajte poboljšane [arhitektura](site-recovery-vmware-to-azure-classic.md#scenario-architecture)i [preduvjetima](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment).
2. Servis za mobilnost Deinstalacija s računala koje trenutno replikaciju. Novu verziju servisa instalirat će se na strojeva kada ih dodati u novu grupu zaštitu.
3. Preuzmite [ključa za registraciju sigurnog](site-recovery-vmware-to-azure-classic.md#step-4-download-a-vault-registration-key) i [pokrenite čarobnjak za postavljanje objedinjenih](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server) za instalaciju poslužitelj za konfiguraciju, postupak server i komponente poslužitelja glavni cilj. Dodatne informacije o [Planiranje kapaciteta](site-recovery-vmware-to-azure-classic.md#capacity-planning).
4. [Postavljanje vjerodajnica](site-recovery-vmware-to-azure-classic.md#step-6-set-up-credentials-for-the-vcenter-server) te oporavak web-mjesta možete koristiti za pristup VMware poslužitelj za automatsko otkrivanje VMware VMs. Saznajte više o [potrebne dozvole](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
5. Dodajte [vCenter poslužitelja ili vSphere domaćini](site-recovery-vmware-to-azure-classic.md#step-7-add-vcenter-servers-and-esxi-hosts). To može potrajati 15 minuta dodatne informacije za poslužitelje da se pojavi na portalu za oporavak web-mjesta.
6. Stvaranje [nove grupe za zaštitu](site-recovery-vmware-to-azure-classic.md#step-8-create-a-protection-group). To može potrajati i do 15 minuta za portal za osvježavanje da bi virtualnim strojevima slučaju otkrivanja i prikazuju se. Ako ne želite čekati možete istaknuti naziv poslužitelja za upravljanje (nemojte kliknuti ga) > **Osvježi**.
7. U odjeljku Zaštita grupi novo kliknite **Migrirati strojeva**.

    ![Dodavanje računa](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)

8. U **Odaberite strojeva** odaberite grupu zaštitu koju želite migrirati iz i računala na koje želite prenijeti.

    ![Dodavanje računa](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)

9. U odjeljku **Postavke za konfiguriranje ciljne** odredite želite li koristi iste postavke za svim računalima, a zatim odaberite poslužitelj za postupak i račun za Azure prostora za pohranu. Ako nemate drugi proces poslužitelja dogodit će se s IP adresu poslužitelja za konfiguraciju poslužitelja.


    ![Dodavanje računa](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [Migracije računa za pohranu](../resource-group-move-resources.md) po grupama resursa unutar iste pretplate ili uzduž pretplate nije podržana za račune za pohranu koji se koriste za implementaciju oporavak web-mjesta.

10. **Odredite računa**odaberite račun koji ste stvorili za poslužitelj za postupak da biste pristupili računalu da biste novu verziju servisa mobilnost.

    ![Dodavanje računa](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)

11. Oporavak web-mjesta neće migrirati repliciranu podataka na račun za Azure prostora za pohranu koju ste naveli. Po želji možete ponovno iskoristiti u prostor za pohranu računa koji ste koristili naslijeđene implementacije.
12. Kada Završi zadatak virtualnim strojevima automatski se sinkroniziraju. Kada se dovrši sinkronizaciju virtualnim strojevima možete izbrisati iz grupe naslijeđene zaštitu.
13. Nakon prenošenja svim računalima možete izbrisati grupi naslijeđene zaštitu.
14. Imajte na umu da biste odredili svojstva prebacivanje strojeva i Azure mrežne postavke nakon sinkronizacije.
15. Ako imate postojeću tarifu za oporavak, možete ih migrirati poboljšane implementaciju s mogućnošću **Migrirati Plan za oporavak** . Učinite to samo kada se prenijela sve zaštićene strojeva. 

    ![Dodavanje računa](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

>[AZURE.NOTE] Nakon što završite migracije nastavak [poboljšane članka](site-recovery-vmware-to-azure-classic.md). Ostatku članka naslijeđene više neće biti odgovarajući i ne morate slijediti korake opisane u it ** više.




## <a name="what-do-i-need"></a>Što moram?

Ovaj dijagram prikazuje komponente implementacije.

![Nove zbirke ključeva](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

Evo što vam je potrebno:

**Komponenta** | **Uvođenje** | **Pojedinosti**
--- | --- | ---
**Poslužitelj za konfiguraciju** | Programa Azure standardne A3 virtualnog računala u okviru iste pretplate kao oporavak web-mjesta. | Poslužitelj za konfiguraciju koordinate komunikaciju između zaštićeni strojeva, poslužitelj za postupak i osnovne ciljnim poslužiteljima u Azure. Postavlja replikacije i oporavak koordinate u Azure kada dođe do prebacivanje.
**Glavni odredišni poslužitelj** | Azure virtualnog računala – ili Windows server koji se temelji na Windows Server 2012 R2 galerije (da biste zaštitili računalima sustava Windows) ili kao Linux server na temelju OpenLogic CentOS 6.6 galerije (da biste zaštitili Linux strojeva).<br/><br/> Tri veličine dostupne su – standardni A4, standardni D14 i standardne DS4.<br/><br/> Poslužitelj je povezano s istom Azure mrežom kao poslužitelj za konfiguraciju.<br/><br/> Postavljanje na portalu za oporavak web-mjesta | Prima i zadržava repliciranu podatke iz svoje zaštićeni strojeva pomoću priložene VHDs stvoreno blobova na vašem računu Azure prostora za pohranu.<br/><br/> Odaberite standardni DS4 posebno za konfiguriranje zaštite radnih opterećenja koji zahtijevaju dosljedan visoke performanse i niske latencije pomoću računa za pohranu Premium.
**Poslužitelj za postupak** | Lokalni virtualne ili fizička poslužitelj za izvodi Windows Server 2012 R2<br/><br/> Preporučujemo da se smješta na istoj mreži i segmentu LAN-a kao strojevima koji želite zaštititi, ali možete pokrenuti na drugoj mreži dok god zaštićeni računalima imati L3 vidljivost mreže da biste ga.<br/><br/> Postavili ste ga, a registrirali za poslužitelj za konfiguraciju na portalu za oporavak web-mjesta. | Zaštićeni strojeva slati podatke replikacije lokalni poslužitelj za postupak. Ima temeljenog na disku replikacije predmemorije u predmemoriji podataka koji ga Prima. Broj akcija izvodi na tih podataka.<br/><br/> Zato optimizira podataka predmemorije, sažimanje i šifriranje prije slanja na poslužitelj glavni cilj.<br/><br/> Obrađuje automatske instalacije servisa mobilnost.<br/><br/> Automatsko otkrivanje VMware virtualnim strojevima izvodi.
**Lokalnog računala** | Lokalni VMware virtualnim strojevima ili fizičke poslužiteljima sa sustavom Windows ili Linux. | Konfiguriranje postavki replikacije koji se odnose na jednu ili više računala. Koje uspijeva putem pojedinačna računala ili više najčešće više računala zajedno prikupite u plan za oporavak. 
**Servis za mobilnost** | Instalirana na svakom virtualnog računala ili fizičke poslužitelja koji želite zaštititi<br/><br/> Mogu ručno instalirati ili pomiču i automatski instalirao poslužitelj za postupak Kada omogućite replikacije za računalo. | Servis za mobilnost šalje podatke na poslužitelj za postupak tijekom početne replikacije (resync). Kada je računalo u zaštićeno stanje (završetku resync) servis mobilnost spremit će upisivanje disk u memoriji i im šalje na poslužitelj za postupak. Applicationconsistency za Windows poslužitelje je postići pomoću VSS.
**Azure sigurnog oporavak web-mjesta** | Stvaranje zbirke ključeva za oporavak web-mjesta s Azure pretplate i registrirati poslužitelji u zbirke ključeva. | Na sigurnog koordinate i orchestrates replikacije podataka, prebacivanje i oporavak između lokalnog web-mjesta i Azure.
**Mehanizam replikacije** | **Putem Interneta**– Communicates i umnožava podatke iz zaštićenog lokalne poslužitelje Azure pomoću sigurne SSL/TLS kanal putem Interneta. Ovo je zadana mogućnost.<br/><br/> **VPN-ExpressRoute**– Communicates i umnožava podataka između lokalne poslužitelje i Azure putem VPN veza. Morat ćete postaviti web-mjesto VPN-a ili je ExpressRoute veza između lokalnog web-mjesta i Azure mreže.<br/><br/> Odabrat ćete način za replikaciju tijekom implementacije oporavak web-mjesta. Kada je konfiguriran bez utjecaja replikacije postojeće strojeva ne možete promijeniti u mehanizam. | Nijedna mogućnost morate otvarati nikakve priključke ulaznog mreže na zaštićenom. Pokreće sve mrežnu komunikaciju s lokalnim web-mjesta. 

## <a name="capacity-planning"></a>Planiranje kapaciteta

Glavna područja koja je potrebno uzeti u obzir:

- **Okruženje za izvor**– infrastrukture u VMware, izvorne postavke računala i preduvjeti.
- **Poslužitelje komponente**– poslužitelj za postupak, poslužitelj za konfiguraciju i osnovne odredišni poslužitelj 

### <a name="considerations-for-the-source-environment"></a>Razmatranja okruženju izvora

- **Maksimalna veličina diska**– trenutni Maksimalna veličina disk koji se može priložiti virtualnog računala je 1 TB. Stoga maksimalne veličine izvorni disk koji je moguće je replicirati i ograničeno je na 1 TB.
- **Maksimalna veličina po izvoru**– maksimalne veličine stroj jedan izvor je 31 Terabajta (s diskova 31) i pomoću D14 instance dodjeli za osnovne odredišni poslužitelj. 
- **Broj izvora po osnovne ciljnom poslužitelju**– više izvora računala možete zaštititi i s poslužiteljem jedne osnovne cilj. Međutim, stroj jedan izvor ne može zaštićeni preko više osnovne ciljnim poslužiteljima jer kao što je replicirati diskova, VHD koja preslikava veličinu diska stvoreno blobova platforme Azure sustavu i priloženim kao podatkovni disk na poslužitelj za osnovne cilj.  
- **Svakodnevno Najveća promjena brzina po izvoru**– postoje tri čimbenici koje treba smatrati kada odlučuje brzina preporučene izmjene po izvoru. Imajte na umu cilj temelji dva IOPS potrebni su Ciljni disk za svaku operaciju na izvorišnog web-mjesta. To je zato čitanje stare podatke i pisanje novih podataka će se dogoditi na disku cilj. 
    - **Dnevnu promjena stopa postupak poslužitelj podržava**– izvornog računala ne može obuhvaćati više postupak poslužitelja. Poslužitelj za jedan proces podržavaju do 1 TB dnevnih promjena stopa. Dakle 1 TB je maksimalna dnevno podaci mijenjaju stopa podržana za izvornog računala. 
    - **Maksimalno protok podržava Ciljni disk**– maksimalno churn po izvorni disk ne može biti više od 144 GB/dan (veličine 8 K pisanje). Pogledajte tablicu u odjeljku glavni cilj za propusnost i IOPs cilja za različite pisanje veličine. Taj broj mora biti podijeljen dva jer se svaki izvor IOP generira 2 IOPS na disku cilj. Saznajte više o [Azure skalabilnost i performanse ciljnih web-mjesta](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) prilikom konfiguriranja cilj račune za premium prostora za pohranu.
    - **Maksimalna propusnost račun za pohranu podržava**– izvor ne može obuhvaćati više prostora za pohranu računa. Zadanu s računom za pohranu potrebno je maksimalno 20 000 za u sekundi i da svaki izvor IOP generira 2 IOPS na glavni odredišni poslužitelj, preporučujemo da ostavite broj IOPS preko izvor do 10 000. Saznajte više o [Azure skalabilnost i performanse ciljnih web-mjesta](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) prilikom konfiguriranja izvor račune za premium prostora za pohranu.

### <a name="considerations-for-component-servers"></a>Zahtjevi za poslužitelje komponente

Tablica 1 navedene veličine virtualnog računala za konfiguraciju i osnovne ciljnim poslužiteljima.

**Komponenta** | **Distribuiranih Azure instanci** | **Jezgri** | **Memorije** | **Max diskova** | **Veličina diska**
--- | --- | --- | --- | --- | ---
Poslužitelj za konfiguraciju | Standardni A3 | 4 | 7 GB | 8 | 1023 GB
Glavni odredišni poslužitelj | Standardni A4 | 8 | 14 GB | 16 | 1023 GB
 | Standardni D14 | 16 | 112 GB | 32 | 1023 GB
 | Standardni DS4 | 8 | 28 GB | 16 | 1023 GB

**Tablica 1**

#### <a name="process-server-considerations"></a>Razmatranja procesa poslužitelja

Općenito promjenu veličine poslužitelj za postupak ovisi o dnevnih stopa promjena preko svih radnih opterećenja zaštićeni.


- Potreban vam je dovoljno računalnim izvršavanje zadataka kao što je web-mjesto u istoj razini sažimanja i u okvir za šifriranje.
- Poslužitelj za postupak koristi predmemorije diska temelje. Provjerite je li preporučene predmemorije prostora i dostupna da biste olakšali promjene podataka pohranjene u slučaju usko grlo mreže ili nedostupnosti je propusnost diska. 
- Da bi poslužitelj za postupak možete prenijeti podatke s poslužiteljem osnovne cilj omogućuju neprekidnu zaštitu podataka provjerite je li dovoljno propusnosti. 

Tablica 2 daje sažetak smjernicama poslužitelja postupak.

**Podaci mijenjaju stopa** | **PROCESOR** | **Memorije** | **Veličina predmemorije na disku**| **Predmemorija propusnost diska** | **Ingress izlazne propusnosti**
--- | --- | --- | --- | --- | ---
< 300 GB | 4 vCPUs (2 sockets * 2 jezgri @ GHz 2,5) | 4 GB | 600 GB | 7 do 10 MB u sekundi | 30 MB/s-21 MB/s
GB 300 600 | 8 vCPUs (2 sockets * 4 jezgri @ GHz 2,5) | 6 GB | 600 GB | MB 11 do 15 sekundi | 60 MB/s-42 MB/s
600 GB 1 TB | 12 vCPUs (2 sockets * 6 jezgri @ GHz 2,5) | 8 GB | 600 GB | 16 do 20 MB u sekundi | 100 MB/s-70 MB/s
> 1 TB | Drugi poslužitelj za postupak implementacije | | | | 

**Tablica 2**

Pri čemu je: 

- Ingress je preuzimanje propusnosti (intranet između poslužitelja izvor i proces).
- Izlazne je prijenos propusnosti (internet između poslužitelja postupak i osnovne odredišni poslužitelj). Brojevi izlazne presume average sažimanja 30% postupak poslužitelja.
- Predmemoriju diska zaseban disk OS minimalne 128 GB preporučuje se za sve poslužitelje postupak.
- Sljedeće prostora za pohranu za propusnost diska predmemorije je korišten za benchmarking: 8 SAS pogona od 10 K RPM s konfiguracijom RAID 10.

#### <a name="configuration-server-considerations"></a>Razmatranja poslužitelja za konfiguraciju

Svaki poslužitelj za konfiguraciju podržavaju do 100 izvora računalima s 3-4 količine. Ako je implementaciju sustava veća preporučujemo da implementacija drugi poslužitelj za konfiguraciju. Zadana svojstva virtualnog računala poslužitelja za konfiguraciju potražite u članku tablica 1. 

#### <a name="master-target-server-and-storage-account-considerations"></a>Glavni cilj poslužitelj i pohranu računa pitanja vezana uz

Prostor za pohranu za svaki poslužitelj osnovne ciljne obuhvaća na disk OS, zadržavanja glasnoću i diskova podataka. Pogon zadržavanja održava dnevnik promjene na disku trajanja prozora zadržavanja definiran na portalu za oporavak web-mjesta.  Pogledajte tablica 1 za svojstva virtualnog računala poslužitelja glavni cilj. Tablica 3 prikazuje kako se koriste diskova A4.

**Instance** | **OS disk** | **Zadržavanja** | **Diskova podataka**
--- | --- | --- | ---
 | | **Zadržavanja** | **Diskova podataka**
Standardni A4 | 1 na disku (1 * 1023 GB) | 1 na disku (1 * 1023 GB) | 15 diskova (15 * 1023 GB)
Standardni D14 |  1 na disku (1 * 1023 GB) | 1 na disku (1 * 1023 GB) | 31 diskova (15 * 1023 GB)
Standardni DS4 |  1 na disku (1 * 1023 GB) | 1 na disku (1 * 1023 GB) | 15 diskova (15 * 1023 GB)

**Tablica 3**

Planiranje za osnovne odredišni poslužitelj kapaciteta ovisi o:

- Performanse Azure prostora za pohranu i ograničenja
    - Maksimalni broj iznimno koristi diskova za standardne VM sloju, otprilike 40 (20 000/500 IOPS po disk) u prostor za pohranu za jedan račun. Saznajte više o [skalabilnost ciljevi za sccounts standardne prostora za pohranu](../storage/storage-scalability-targets.md#scalability-targets-for-standard-storage-accounts) i [sccounts premium prostora za pohranu](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts).
-   Svakodnevno promjena stopa 
-   Prostor za pohranu glasnoću zadržavanja.

Imajte na umu da:

- Jedan izvor ne može se protežu na više prostora za pohranu računa. To se odnosi na disku podataka s računima za pohranu odabrali prilikom konfiguriranja zaštitu. OS-a i diskova zadržavanja obično idite na karticu računa automatski distribuiranih prostora za pohranu.
- Jedinice za pohranu zadržavanja potrebna ovisi o dnevnih promjena stopu i broj dana zadržavanja. Zadržavanja spremište potrebno po osnovne odredišni poslužitelj = Ukupno churn iz izvora dnevno * broj dana zadržavanja. 
- Svaki osnovne odredišni poslužitelj sadrži samo jednu zadržavanja jedinicu. Glasnoću zadržavanja se zajednički koristi na diskova priložiti osnovne odredišni poslužitelj. Ako, na primjer:
    - Ako je izvor računalo s 5 diskova te svakom disku generira 120 IOPS (8K veličina) na izvorišnog web-mjesta, to prevodi 240 IOPS po disk (2 operacije na disku cilj po izvoru IO). 240 IOPS je unutar Azure po disk IOPS ograničenje od 500.
    - Na opseg zadržavanja, to će biti 120 * 5 = 600 IOPS, i to može postati boca vratu. U ovom scenariju dobra strategija bi da biste dodali više diskova glasnoće zadržavanja i obuhvaćaju ga preko, kao RAID pruga konfiguraciju. To će poboljšati performanse jer su na IOPS raspodijeliti više pogona. Broj pogona dodati glasnoće zadržavanja bit će na sljedeći način:
        - Ukupno IOPS iz izvora okruženja / 500
        - Ukupna churn dnevno iz okruženja izvora (dekomprimirati) / 287 GB. 287 GB je maksimalna protok podržava Ciljni disk prema danu. Ova metrika će razlikuju se ovisno o veličinu pisanje uz faktor 8 k jer je u tom slučaju 8 je K tobom pretpostavlja da je veličina za pisanje. Na primjer, ako je veličina pisanje 4K zatim propusnost bit će 287/2. A ako je veličina pisanje 16K zatim propusnost bit će 287 * 2.
- Broj računa za pohranu potrebno = izvora zbroja IOPs/10000.


## <a name="before-you-start"></a>Prije početka

**Komponenta** | **Preduvjeti** | **Pojedinosti**
--- | --- | --- 
**Račun za Azure** | Potreban vam je račun sustava [Microsoft Azure](https://azure.microsoft.com/) . Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
**Azure prostora za pohranu** | Morat ćete račun Azure prostora za pohranu za pohranu repliciranu podataka<br/><br/> Na račun mora biti u standardni suvišnih zemlj za pohranu ili [Račun](../storage/storage-redundancy.md#geo-redundant-storage) [Premium prostora za pohranu](../storage/storage-premium-storage.md).<br/><br/> Morate u području isti kao servisa Azure oporavak web-mjesta, a biti pridruženi iste pretplate. Ne podržavamo Premjesti račune za pohranu stvoren pomoću [Novi Azure portal](../storage/storage-create-storage-account.md) preko grupe resursa.<br/><br/> Dodatne informacije potražite u članku [Uvod u spremište na platformi Microsoft Azure](../storage/storage-introduction.md)
**Azure virtualne mreže** | Potreban vam je na kojem poslužitelj za konfiguraciju i osnovne odredišni poslužitelj će biti implementirano Azure virtualne mreže. Mora biti u istom pretplate i regija kao sigurnog oporavak Azure web-mjesta. Ako želite replicirati podataka putem veze ExpressRoute ili VPN-a Azure virtualne mreže morate biti povezani s mrežom na lokalni putem veze s ExpressRoute ili web-mjesto VPN-a.
**Resursi za Azure** | Provjerite je li dovoljno Azure resurse za implementaciju sve komponente. Pročitajte više u [Ograničenja Azure pretplate](../azure-subscription-service-limits.md).
**Azure virtualnim strojevima** | Virtualnim strojevima koji želite zaštititi mora biti u skladu s [Azure preduvjeti](site-recovery-best-practices.md).<br/><br/> **Na disku count**– najviše 31 diskova mogu podržavati na jednom zaštićeni poslužitelju<br/><br/> **Na disku veličine**– kapacitetu pojedinačne diska bi trebalo biti više od 1023 GB<br/><br/> **Clustering**– poslužitelja u grupirani nisu podržane<br/><br/> **Pokretanje**– Sjedinjene Extensible firmver sučelja (UEFI) / Extensible Interface(EFI) firmver pokretanje nije podržana<br/><br/> **Količine**– Bitlocker šifrirane količine nisu podržane<br/><br/> **Nazivi poslužitelja**– imena mora sadržavati između 1 i 63 znakova (slova, brojeve i spojnice). Naziv mora započeti s slovo ili broj i završavaju slovo ili broj. Kada je zaštićena stroj možete izmijeniti naziv Azure.
**Poslužitelj za konfiguraciju** | Standardni A3 virtualnog računala koji se temelji na slici Galerija Azure web-mjesta oporavka sustava Windows Server 2012 R2 stvorit će se u svoju pretplatu na poslužitelj za konfiguraciju. Stvara se kao prvu instancu u novi u oblaku. Ako odaberete javni Internet kao vrstu s povezivanjem poslužitelja za konfiguraciju servisa u oblaku stvorit će se s rezerviranim javnu IP adresu.<br/><br/> Put instalacije mora biti u engleske znakove.
**Glavni odredišni poslužitelj** | Virtualnog računala Azure, standardne A4, D14 ili DS4.<br/><br/> Put instalacije mora biti u engleske znakove. Na primjer put mora biti **/usr/local/ASR** za osnovne cilj poslužitelju koji se izvodi Linux.
**Poslužitelj za postupak** | Možete implementirati poslužitelju postupak za fizičke ili virtualnog računala koji se izvodi Windows Server 2012 R2 s najnovijim ažuriranjima. Instalacija na C: /.<br/><br/> Preporučujemo da postavite poslužitelj na mreži i podmreže kao strojeva koji želite zaštititi.<br/><br/> Instalirajte VMware vSphere EŽA 5.5.0 na poslužitelj za postupak. Da biste otkrili virtualnim strojevima upravlja vCenter poslužitelja ili virtualnim strojevima koji se izvode na glavno računalo za ESXi potreban je komponentu VMware vSphere EŽA na poslužitelju postupak.<br/><br/> Put instalacije mora biti u engleske znakove.<br/><br/> ReFS datotečni sustav nije podržan.
**VMware** | VCenter VMware poslužitelj upravljanje vaše hypervisors vSphere VMware. Ga treba imati vCenter verzija 5.1 ili 5,5 s najnovijim ažuriranjima.<br/><br/> Jedan ili više hypervisors vSphere koja sadrži VMware virtualnim strojevima koji želite zaštititi. Na hypervisor mora biti pokrenut ESX/ESXi verzije 5.1 ili 5,5 s najnovijim ažuriranjima.<br/><br/> VMware virtualnim strojevima moraju imati instaliran i pokrenut Alati za VMware. 
**Strojevima sa sustavom Windows** | Zaštićeni fizičke poslužitelja ili VMware virtualnim strojevima sa sustavom Windows imaju broj preduvjeti.<br/><br/> A podržane 64-bitni operacijski sustav: **Windows Server 2012 R2**, **Windows Server 2012**ili **Windows Server 2008 R2 sa pri najmanje SP1**.<br/><br/> Naziv glavnog računala, točke pogona, nazivi uređaj, put sustava Windows (npr: C:\Windows) mora biti na engleskom samo.<br/><br/> Operacijski sustav trebala biti instalirana na pogon C:\.<br/><br/> Podržani su samo osnovnih diskova. Dinamičkih diskova nisu podržane.<br/><br/> Pravila vatrozida na zaštićenom dopustite da dođete do ciljnim poslužiteljima konfiguraciju i matrica u Azure.p ><p>Morat ćete unijeti administratorski račun (morate biti administrator lokalne na računalu Windows) za instalaciju automatske servis mobilnost na poslužiteljima sustava Windows. Ako je račun za navedeni račun koji nije domene morat ćete onemogućiti kontrolu za daljinski pristup korisnika na lokalnom računalu. Da biste učinili to dodajte stavku registra LocalAccountTokenFilterPolicy DWORD s vrijednost 1 u odjeljku HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. Da biste dodali unos registra iz otvorene cmd EŽA ili powershell i upišite **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. [Saznajte više](https://msdn.microsoft.com/library/aa826699.aspx) o kontrola pristupa.<br/><br/> Nakon prebacivanje, ako želite povezati s virtualnim računalima sustava Windows u Azure s udaljene radne površine provjerite udaljene radne površine omogućena za na lokalno računalo. Ako se ne povezujete putem VPN, pravila vatrozida dopustite veze udaljene radne površine putem Interneta.
**Linux računalima** | Podržani 64-bitni operacijski sustav: **Centos 6,4 6.5, 6.6**; **Linux 6,4 tvrtke oracle, 6.5 pokrenut je vaša crveno kompatibilne otklanjanje ili Unbreakable Enterprise otklanjanje izdanje 3 (UEK3)**, **SUSE Linux Enterprise Server 11 SP3**.<br/><br/> Pravila vatrozida na zaštićenom dopustite da dođete do konfiguracija i osnovne ciljnim poslužiteljima u Azure.<br/><br/> /etc/Hosts datoteke na zaštićenom moraju sadržavati stavke koje mapiranje lokalne glavno IP adrese pridružene sve NIC-ovi <br/><br/> Ako želite povezati Azure virtualnog računala radi Linux nakon prebacivanje pomoću ljuske za sigurnu klijenta (ssh) provjerite je li servis za sigurnu ljuske na zaštićenom računalu postavljena da biste započeli na automatsko pokretanje sustava i dopustiti pravila vatrozida programa ssh veze na njega.<br/><br/> Naziv glavnog računala, točke pogona, nazivi uređaja i Linux sustava putovima i nazivima datoteka (npr/itd /; /usr) mora biti na engleskom samo.<br/><br/> Zaštita je moguće omogućiti za lokalni strojevima sa sljedećim prostora za pohranu:-<br>Datotečni sustav: EXT3, ETX4, ReiserFS, XFS<br>Multipath softver uređaji mapiranje (multipath)<br>Upravitelj glasnoće: LVM2<br>Fizička poslužitelji za pohranu kontroler HP CCISS nisu podržane.
**Drugih proizvođača** | Neke komponente implementacije u ovom scenariju ovise o proizvođača softvera pravilno funkcionirati. Potpuni popis potražite u članku [obavijesti trećih strana softver i podataka](#third-party)


### <a name="network-connectivity"></a>Veza s mrežom

Imate dvije mogućnosti prilikom konfiguriranja mrežne veze između lokalno mjesto i Azure virtualne mreže uvode komponente infrastrukture (poslužitelj za konfiguraciju, osnovne ciljnim poslužiteljima). Morat ćete odlučiti koju mogućnost povezivanje s mrežom koristiti prije možete implementirati poslužitelj za konfiguraciju. Morat ćete odabrati tu postavku u trenutku implementacije. To nije moguće promijeniti kasnije.

**Internet:** Komunikacija i replikacije podataka između lokalne poslužitelje (poslužitelj za postupak, zaštićeni strojeva) i poslužitelje komponente Azure infrastrukture (poslužitelj za konfiguraciju, osnovne odredišni poslužitelj) događa putem sigurne SSL/TLS veze s lokalnim da biste javno krajnje točke ciljnim poslužiteljima konfiguracija i matrice. (Jedina je iznimka veze između poslužitelj za postupak i osnovne odredišni poslužitelj na priključak TCP 9080 koji je šifrirana. Kontrola samo informacije vezane uz replikacije protokol za postavljanje replikacije izmjenjuju na tu vezu.)

![Uvođenje dijagram internet](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**VPN**: komunikacija i replikacije podataka između lokalne poslužitelje (poslužitelj za postupak, zaštićeni strojeva) i poslužitelje komponente Azure infrastrukture (poslužitelj za konfiguraciju, osnovne odredišni poslužitelj) se događa putem VPN vezu između lokalne mreže i Azure virtualne mreže na kojima su implementiran poslužitelj za konfiguraciju i osnovne ciljnim poslužiteljima. Provjerite je li da lokalne mreže povezani Azure virtualne mrežu tako da je veza ExpressRoute ili VPN veza web-mjesto.

![Dijagram implementacije VPN-a](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)


## <a name="step-1-create-a-vault"></a>Korak 1: Stvaranje na zbirke ključeva

1. Prijava na [Portal za upravljanje](https://portal.azure.com).


2. Proširite **Data Services** > **Oporavak usluge** kliknite **Sigurnog oporavak web-mjesta**.


3. Kliknite **Stvori novi** > **brzo stvaranje**.

4. U odjeljak **naziv**unesite neslužbeni naziv da biste odredili na zbirke ključeva.

5. U **regiji**, odaberite regiji u zbirke ključeva. Da biste provjerili podržanih regija pogledajte geografske dostupnost u [Azure web-mjesta oporavak cijene detalja](https://azure.microsoft.com/pricing/details/site-recovery/)

6. Kliknite **Stvaranje sigurnog**.

    ![Nove zbirke ključeva](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Provjerite traku stanja da biste potvrdili na sigurnog uspješno je stvorena. Na sigurnog će biti navedena kao **aktivna** na glavnoj stranici **Servisa za oporavak** .

## <a name="step-2-deploy-a-configuration-server"></a>Korak 2: Implementacija poslužitelja za konfiguraciju

### <a name="configure-server-settings"></a>Konfiguriranje postavki poslužitelja

1. Na stranici **Servisa oporavak** kliknite sigurnog da biste otvorili stranicu za brzo pokretanje. Brzi početak rada može se otvoriti u bilo kojem trenutku pomoću ikone.

    ![Ikona za brzi početak rada](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)

2. Na padajućem popisu odaberite **između lokalnog web-mjesta s poslužiteljima VMware/fizičke i Azure**.
3. **Priprema Target(Azure)** resursa kliknite **Implementacija poslužitelja za konfiguraciju**.

    ![Implementacija poslužitelja za konfiguraciju](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)

4. U **Novim detaljima o konfiguraciji poslužitelja** navedite:

    - Naziv poslužitelja za konfiguraciju i vjerodajnice za povezivanje s njom.
    - U vrsti povezivanje s mrežom padajućeg izbornika odaberite **Javnog Interneta** ili **VPN-a**. Imajte na umu da ne možete promijeniti ovu postavku kada se primjenjuje.
    - Odaberite Azure mrežu na kojoj treba nalazi na poslužitelju. Ako koristite VPN-a provjerite sigurni Azure mreže povezano s lokalne mreže prema očekivanjima. 
    - Odredite internu IP adresa i podmreže koji će se dodijeliti na poslužitelj. Imajte na umu da se prve četiri IP adresa u bilo kojem podmreže rezervirana za internu upotrebu Azure. Pomoću bilo koje druge dostupne IP adrese.
    
    ![Implementacija poslužitelja za konfiguraciju](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)

5. Kada kliknete **u redu** u standardni A3 virtualnog računala koji se temelji na slici Galerija Azure web-mjesta oporavka sustava Windows Server 2012 R2 stvorit će se u svoju pretplatu na poslužitelj za konfiguraciju. Stvara se kao prvu instancu u novi u oblaku. Ako ste odabrali da biste se povezali putem Interneta servisa u oblaku stvara se pomoću rezervirane javnu IP adresu. Možete pratiti napredak na kartici **Zadaci** .

    ![Nadzor napretka](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)

6.  Ako se povezujete putem Interneta, nakon poslužitelj za konfiguraciju distribuiranih bilješke na javnu IP adresu joj je dodijeljena na stranici **virtualnim strojevima** na portalu za Azure. Zatim na **krajnje točke** kartica bilješke javno priključak HTTPS mapirati privatne priključak 443. Morat ćete podatke kasnije kada registrirate osnovne cilj i proces poslužitelja s poslužitelja za konfiguraciju. Poslužitelj za konfiguraciju uvode se sa te krajnje točke:

    - HTTPS: Javno priključak služi za koordiniranje komunikaciju između poslužitelje komponente i Azure putem Interneta. Privatni priključak 443 koristi se za koordiniranje komunikaciju između poslužitelje komponente i Azure putem VPN-a.
    - Prilagođeno: Javno priključak koristi se za failback alat za komunikaciju putem Interneta. Privatni priključak 9443 koristi failback alat za komunikaciju putem VPN-a.
    - PowerShell: Privatni priključak 5986
    - Udaljena radna površina: privatne priključak 3389
    
    ![VM krajnje točke](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

    >[AZURE.WARNING] Ne, izbrišite ili promijenite broj priključka javna ili privatna sve krajnje točke stvoren tijekom implementacija poslužitelja za konfiguraciju.

Poslužitelj za konfiguraciju je uveden u se automatski stvoreni Azure u oblaku s rezerviranim IP adresa. Rezervirane adresa je potrebno da biste bili sigurni da konfiguraciju poslužitelja oblaka servisa IP adresa ostaje preko ponovna virtualnih računala (uključujući poslužitelj za konfiguraciju) na servis u oblaku. Rezervirane javnu IP adresu bit će potrebno ručno dostatne kada je prestanka korištenja računala poslužitelja za konfiguraciju ili će ostati rezervirane. Postoji zadano ograničenje od 20 rezervirane javnu IP adrese po pretplati. [Saznajte više](../virtual-network/virtual-networks-reserved-private-ip.md) o Rezervirana IP adresa. 

### <a name="register-the-configuration-server-in-the-vault"></a>Registriranje poslužitelja za konfiguraciju u na zbirke ključeva

1. Na stranici za **Brzo pokretanje** kliknite **Priprema ciljnog resursi** > **Preuzimanje ključa za registraciju**. Automatski se generira ključne datoteka. Nije valjan 5 dana nakon generira. Kopirajte ga na poslužitelj za konfiguraciju.
2. Na **virtualnim računalima sustava** odaberite poslužitelj za konfiguraciju na popisu virtualnih računala. Otvorite karticu **nadzorne ploče** , a zatim kliknite **Poveži**. **Otvaranje** preuzetu datoteku RDP prijavite se na poslužitelj za konfiguraciju putem udaljene radne površine. Ako koristite VPN, koristite interne IP adrese (adresu koju ste naveli prilikom implementiran poslužitelj za konfiguraciju) udaljene radne površine veze s lokalnim web-mjesta. Čarobnjak za postavljanje paketa Azure web-mjesta oporavak konfiguraciju poslužitelja automatski se pokreće kada se prvi put prijavite.

    ![Registracija](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)

3. **Instalacija softvera drugih proizvođača** kliknite **prihvaćam** da biste preuzeli i instalirali MySQL.

    ![Instalacija MySQL](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)

4. Stvorite vjerodajnice za prijavu na instanca poslužitelja MySQL u **Detalji o poslužitelju MySQL** .

    ![Vjerodajnice MySQL](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)

5. U odjeljku **Postavke internetske** odredite kako će se poslužitelj za konfiguraciju povezati s Internetom. Imajte na umu da:

    - Ako želite koristiti prilagođenu proxy ga trebali postaviti prije instalacije davatelja usluga.
    - Kada kliknete **Dalje** test će se pokrenuti da biste provjerili proxy vezu.
    - Ako koristite prilagođeni proxy ili zadani proxy zahtijeva provjeru autentičnosti morat ćete unijeti detalje proxy poslužitelj, uključujući adresu, priključak i vjerodajnice.
    - Sljedeći URL-ovi mora biti dostupno putem proxy poslužitelja:
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Ako imate IP adresa sustavom pravila vatrozida provjerite je li pravila postavljene dopustiti komunikaciju s poslužitelja za konfiguraciju IP adrese opisane u [Azure podatkovnog centra IP rasponi](https://msdn.microsoft.com/library/azure/dn175718.aspx) i HTTPS protokola (443). Promijenile bijeli popis IP rasponi Azure regije koje namjeravate koristi i koje Zapad SAD-a.

    ![Registracija proxy poslužitelja](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)

6. U **Odjeljku postavke davatelja lokalizaciju poruku pogreške** navedite jezik koji želite da se prikazuju poruke o pogreškama.

    ![Registracija poruka o pogrešci](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)

7. U **Registraciju za oporavak Azure web-mjesta** pronađite i odaberite ključa datoteku koju ste kopirali na poslužitelj.

    ![Registracija datoteka s ključem](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)

8. Na stranici dovršetak čarobnjaka odaberite ovih mogućnosti:

    - Odaberite **Pokretanje upravljanje u dijaloškom okviru računa** da biste odredili dijaloški okvir upravljanje računima otvarajte nakon što završite čarobnjak.
    - Odaberite **Stvori ikonu na radnoj površini Cspsconfigtool** da biste dodali prečac radne površine na poslužitelj za konfiguraciju tako da otvorite dijaloški okvir **Upravljanje računima** u bilo kojem trenutku bez obzira na to ponovno pokrenite čarobnjak.

    ![Dovršavanje Registracija](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)

9. Kliknite **Završi** da biste dovršili Čarobnjak. Pristupni izraz se generira. Kopirajte ga na sigurnom mjestu. Potreban vam je za provjeru autentičnosti i registrirati postupak i osnovne ciljnim poslužiteljima s poslužitelja za konfiguraciju. Koristi se i u cilju integriteta kanala u konfiguraciji poslužitelja komunikacije. Možete Obnovi pristupni izraz, ali zatim morat ćete ponovno registrirajte osnovne cilj i obrada poslužitelja koja koristi novi pristupni izraz.

    ![Pristupni izraz](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Nakon registracije poslužitelja za konfiguraciju će se prikazati na stranici **Konfiguracija poslužitelja** u na sigurnog.

### <a name="set-up-and-manage-accounts"></a>Postavljanje i upravljanje računima

Tijekom implementacije oporavak web-mjesta zahtijeva vjerodajnice za sljedeće radnje:

- VMware računom thatSite oporavak možete automatski otkrivanje VMs na poslužiteljima vCenter ili vSphere domaćini. 
- Kada dodate strojeva za zaštitu, tako da se oporavak web-mjesta servisa mobilnost možete instalirati na njima.

Nakon što ste registrirali poslužitelj za konfiguraciju možete otvoriti dijaloški okvir **Upravljanje računima** da biste dodali i upravljanje računima koji želite koristiti za ove akcije. Nekoliko je načina da biste to učinili:

- Otvorite prečac ste odlučili stvorene za dijaloški okvir na zadnju stranicu postavke poslužitelja za konfiguraciju (cspsconfigtool).
- Otvorite dijaloški okvir Završi postavljanje konfiguraciju poslužitelja.

1. **Upravljanje** računima kliknite **Dodaj račun**. Izmjena i brisanje postojećih računa.

    ![Upravljanje računima](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)

2. U **Pojedinosti o računu** Navedite naziv računa za korištenje u Azure i vjerodajnice (domena/korisničko ime). 

    ![Upravljanje računima](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-to-the-configuration-server"></a>Povezivanje s poslužitelja za konfiguraciju 

Povezivanje s poslužitelja za konfiguraciju na dva načina:

- Putem s VPN-a web-mjesta-na-web-mjesta ili ExpressRoute
- Putem Interneta 

Imajte na umu da:

- Internetska veza koristi krajnjih točaka virtualnog računala u kombinaciji s javnim virtualne IP adresu poslužitelja.
- VPN veza koristi internu IP adresu poslužitelja zajedno s priključke privatne krajnjoj točki.
- Jednokratno odluka odlučite li se povezati (kontrole obrazaca ni replikacije podataka) iz lokalne poslužitelje je na razne poslužitelje komponente (poslužitelj za konfiguraciju, osnovne odredišni poslužitelj) radi u Azure putem VPN vezu ili Internetu. Ne možete promijeniti ovu postavku naknadno. Ako vidite morat ćete ponovno implementirate scenarij i reprotect vašeg računala.  


## <a name="step-3-deploy-the-master-target-server"></a>Korak 3: Implementacija poslužitelja osnovne cilj

1. Kliknite **Resursi za pripremu Target(Azure)** > **uvođenja osnovne odredišni poslužitelj**.
2. Navedite Detalji o poslužitelju za osnovne cilj i vjerodajnice. Poslužitelj će biti implementirano u istom Azure mrežom kao poslužitelj za konfiguraciju. Kada kliknete da biste dovršili Azure virtualnog računala stvorit će se sa slikom Galerija Windows i Linux.

    ![Postavke poslužitelja cilj](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Imajte na umu da se prve četiri IP adresa u bilo kojem podmreže rezervirana za internu upotrebu Azure. Navedite bilo koje druge dostupne IP adrese.

>[AZURE.NOTE] Odaberite standardni DS4 prilikom konfiguriranja zaštite radnih opterećenja koji zahtijevaju dosljedan visoke/i performanse i niske latencije da bi se hostira/i intenzivno radnih opterećenja pomoću [Računa za pohranu Premium](../storage/storage-premium-storage.md).


3. Sa sustavom Windows osnovnih poslužitelju ciljni VM stvara se pomoću te krajnje točke. Imajte na umu da javne krajnje točke stvaraju se samo ako povezivanja putem Interneta.

    - Prilagođeno: Javno priključak koristi poslužitelj za postupak da biste poslali replikacije podataka putem Interneta. Slanje podataka replikacije u glavni odredišni poslužitelj putem VPN-a koristi se privatni priključak 9443 poslužitelj za postupak.
    - Custom1: Javno priključak koristi poslužitelj za postupak da biste poslali metapodataka putem Interneta. Da biste poslali metapodataka osnovne odredišni poslužitelj putem VPN-a koristi se privatni priključak 9080 poslužitelj za postupak.
    - PowerShell: Privatni priključak 5986
    - Udaljena radna površina: privatne priključak 3389

4. Glavni cilj poslužitelju Linux VM stvara se pomoću te krajnje točke. Imajte na umu da javne krajnje točke stvaraju se samo ako se povezujete putem Interneta.

    - Prilagođeno: Javno priključak koristi proces poslužitelj za slanje replikacije podataka putem Interneta. Slanje podataka replikacije u glavni odredišni poslužitelj putem VPN-a koristi se privatni priključak 9443 poslužitelj za postupak.
    - Custom1: Javno priključak koristi poslužitelj za postupak da biste poslali metapodataka putem Interneta. Privatni priključak 9080 koristi poslužitelj za postupak da biste poslali metapodataka osnovne odredišni poslužitelj putem VPN-a
    - SSH: Privatni priključak 22

    >[AZURE.WARNING] Ne brisanje ili promjena javna ili privatna broj priključka bilo koji od krajnje točke stvoren tijekom implementacija poslužitelja glavni cilj.

5. U čekanje **virtualnim strojevima** virtualnog računala da biste pokrenuli.

    - Ako je Windows server bilješku dolje udaljene radne površine detalje.
    - Ako je Linux poslužitelja, a zatim se povezujete putem VPN Imajte na umu interne IP adresu virtualnog računala. Ako se povezujete putem Interneta obratite pažnju na javnu IP adresu.

6.  Prijavite se na poslužitelju da biste dovršili instalaciju, a registrirali s poslužitelja za konfiguraciju. 
7.  Ako koristite operacijski sustav Windows:

    1. Pokretanje udaljene radne površine vezu virtualnog računala. Prvi put prijavite skriptu funkcionirat će u prozoru ljuske PowerShell. Ne zatvorite je. Kada alat za glavno računalo Agent Config otvara se automatski da biste registrirali poslužitelj.
    2. U **Config Agent glavno računalo** odredite internu IP adresu poslužitelja za konfiguraciju i priključak 443. Možete koristiti interne adrese i privatni priključak 443, čak i ako se ne povezujete putem VPN jer virtualnog računala povezan s istom Azure kao poslužitelj za konfiguraciju. Ostavite **HTTPS korištenje** omogućeno. Unesite pristupni izraz za poslužitelj za konfiguraciju koju ste ranije zabilježili. Kliknite **u redu** da biste registrirali poslužitelj. Mogućnosti NAT možete zanemariti. Mogu se koristiti.
    3. Ako je svojim potrebama pogon Procjena zadržavanju više od 1 TB možete konfigurirati glasnoće zadržavanja (R:) pomoću virtualne disku i [prostori za pohranu](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx)
    
    ![Glavni odredišni poslužitelj za Windows](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)

8. Ako koristite Linux:
    1. Provjerite jeste li instalirali na najnovije Linux integraciju servisa (LIS) instaliran prije instalacije osnovne odredišni poslužitelj. Možete pronaći najnoviju verziju LIS te upute za instaliranje [ovdje](https://www.microsoft.com/download/details.aspx?id=46842). Ponovno pokrenite računalo nakon instalacije na LIS.
    2. **Priprema ciljnog (Azure)** resursa kliknite **Preuzmite i instalirajte dodatnim softverom (samo za matricu Linux odredišni poslužitelj)**. Kopiranje datoteke preuzete tar virtualnog računala klijenti sftp. Umjesto toga možete prijaviti na poslužitelj osnovne cilj distribuiranih linux i koristiti *wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* da biste preuzeli na datoteku.
    2. Prijavite se na poslužitelj pomoću ljuske za sigurnu klijenta. Ako ste povezani s mrežom Azure putem VPN-a pomoću interne IP adresa. U suprotnom pomoću vanjske IP adrese i SSH krajnjoj točki javno.
    3. Izdvajanje datoteka iz gzipped instalacijski program tako da pokrenete: **ciljni – xvzf Microsoft-ASR_UA_8.4.0.0_RHEL6-64***
    ![Linux osnovne odredišni poslužitelj](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
    4. Provjerite jeste li u direktoriju koji ste izdvojili sadržaj datoteke tar.
    5. Kopiranje pristupni izraz za konfiguraciju poslužitelja u lokalnu datoteku pomoću naredbe * *echo* `<passphrase>` * > passphrase.txt**
    6. Pokrenite naredbu "**sudo. Instaliraj -t oba - a hostira -R -d /usr/local/ASR MasterTarget -i* `<Configuration server internal IP address>` * -p 443 -s y - c HTTP -P passphrase.txt**".

    ![Registrirajte se poslužitelju ciljni](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)

9. Pričekajte nekoliko minuta (10 do 15), a zatim na stranici Provjera osnovne odredišni poslužitelj nalazi registrirana na **poslužiteljima** > kartice **Detalji o poslužitelju za** **Konfiguraciju poslužitelja** . Ako koristite Linux, a nije registrirala voditelju config alat ponovno pokrenite iz /usr/local/ASR/Vx/bin/hostconfigcli. Morat ćete postaviti dozvole za pristup tako da pokrenete chmod kao korijen.

    ![Provjerite je li poslužitelju ciljni](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

>[AZURE.NOTE] To može potrajati i do 15 minuta nakon registracije za osnovne odredišni poslužitelj navoditi na portalu. Da biste ažurirali odmah, kliknite **Osvježi** na stranici **Konfiguracija poslužitelja** .

## <a name="step-4-deploy-the-on-premises-process-server"></a>Korak 4: Implementacija lokalni poslužitelj za postupak

Preporučujemo da prije nego što počnete da konfigurirate statičke IP adrese na poslužitelju postupak tako da ga je zajamčeno stalni preko ponovna.

1. Kliknite brzi Start > **Instalacija postupak na lokalnom poslužitelju** > **Preuzmite i instalirajte poslužitelj za postupak**.

    ![Instalacija poslužitelj za postupak](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)

2.  Kopirajte zip preuzete datoteke na poslužitelju na kojem ćete instalirati server postupak. Zip datoteka sadrži dvije instalacijskih datoteka:

    - Microsoft-ASR_CX_TP_8.4.0.0_Windows*
    - Microsoft-ASR_CX_8.4.0.0_Windows*

3. Raspakiraj u arhivu i kopirajte instalacijske datoteke na mjesto na poslužitelju.
4. Pokretanje u **Microsoft-ASR_CX_TP_8.4.0.0_Windows*** instalacije datoteka, a zatim pratite upute. To se instalira komponente treće strane koja su potrebna za implementaciju.
5. Pokrenite **Microsoft-ASR_CX_8.4.0.0_Windows***.
6. Na stranici **Način rada poslužitelja** odaberite **Poslužitelj za postupak**.
7. Na stranici **Detalji o okruženju** učinite sljedeće:


    - Ako želite zaštititi VMware virtualnog računala kliknite **da**
    - Ako samo želite zaštititi fizičke poslužitelja, a time ne morate VMware vCLI instalirana na poslužitelju postupak. Kliknite **ne** , a zatim dalje.

8. Prilikom instalacije VMware vCLI Imajte na umu sljedeće:

    - **Podržana je samo VMware vSphere EŽA 5.5.0**. Poslužitelj za postupak ne funkcionira s drugim verzijama ili ažuriranja vSphere EŽA.
    - Preuzmite vSphere EŽA 5.5.0 iz [ovdje.](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
    - Ako ste instalirali vSphere EŽA prije pokretanja instalacije poslužitelj za postupak i instalacijski program ne otkrije, pričekajte do pet minuta prije nego što se ponovno pokušajte instalaciju. Time se osigurava da sve varijable okruženja potrebne za otkrivanje EŽA vSphere ste pokrenuta pravilno.

9.  U **Odabir NIC postupak poslužitelja** odaberite mrežnog prilagodnika koji poslužitelj za postupak koristiti.

    ![Odabir prilagodnika](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)

10. U **konfiguraciji poslužitelja detalje**:

    - Za IP adresa i priključaka, ako se povezujete putem VPN odredite internu IP adresu poslužitelja za konfiguraciju i 443 za priključak. U suprotnom navedite javno virtualna IP adresa i mapiranih javno HTTP krajnjoj točki.
    - Upišite pristupni izraz poslužitelja za konfiguraciju.
    - Ako želite onemogućiti provjeru kada koristite automatsko slanje da biste instalirali servis, poništite **mobilnost provjerite je li servis softver potpis** . Provjera potpisa mora internetska veza s poslužiteljem postupak.
    - Kliknite **Dalje**.

    ![Poslužitelj za konfiguraciju REGISTER](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)


11. **Odaberite instalacija** pogon odaberite pogon predmemoriju. Poslužitelj za postupak treba pogon predmemorije s najmanje 600 GB slobodnog prostora. Zatim kliknite **Instaliraj**. 

    ![Poslužitelj za konfiguraciju REGISTER](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)

12. Imajte na umu da možda ćete morati ponovno pokrenuti poslužitelj da biste dovršili instalaciju. U **Konfiguraciji poslužitelja** > **Detalji o poslužitelju** provjerite jesu li poslužitelj za postupak pojavit će se i registriran uspješno u na zbirke ključeva.

>[AZURE.NOTE]To može potrajati i do 15 minuta nakon registracije za poslužitelj za postupak da se prikazuju kao što je navedeno u odjeljku poslužitelj za konfiguraciju. Da biste ažurirali odmah, osvježavanje poslužitelj za konfiguraciju klikom na gumb Osvježi pri dnu stranice za konfiguraciju poslužitelja
 
![Provjera postupak poslužitelja](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Ako niste onemogućivanje Provjera potpisa servisa za mobilnost kada registrirate poslužitelj za postupak to možete učiniti kasnije na sljedeći način:

1. Prijavite se na poslužitelj za postupak kao administrator i otvorite datoteku C:\pushinstallsvc\pushinstaller.conf za uređivanje. U odjeljku **[PushInstaller.transport]** dodajte redak: **SignatureVerificationChecks = "0"**. Spremite i zatvorite datoteku.
2. Ponovno pokrenite servis InMage PushInstall.


## <a name="step-5-update-site-recovery-components"></a>Korak 5: Ažuriranje oporavak web-mjesta komponente

Web-mjesta komponente za oporavak ažuriraju s vremena na vrijeme. Kada novih ažuriranja trebali biste ih instalirajte sljedećim redoslijedom:

1. Poslužitelj za konfiguraciju
2. Poslužitelj za postupak
3. Glavni odredišni poslužitelj
4. Alat za Failback (vContinuum)

### <a name="obtain-and-install-the-updates"></a>Nabavite i instalirajte ažuriranja


1. Ažuriranja u konfiguraciji, postupak i osnovne ciljnim poslužiteljima možete dobiti od oporavak web-mjesta **nadzorne ploče**. Za instalaciju Linux Izdvajanje datoteka iz gzipped instalacijski program, a zatim pokrenite naredbu "sudo. Instaliraj" da biste instalirali ažuriranje.
2. [Preuzmite](http://go.microsoft.com/fwlink/?LinkID=533813) najnovija ažuriranja za Failback tool(vContinuum).
3. Ako koristite virtualnim strojevima ili fizičke poslužiteljima koji već imate instaliran servis za mobilnost biste mogli primati ažuriranja za servis na sljedeći način:

    - **Mogućnost 1**: preuzimanje ažuriranja:
        - [Windows Server (samo 64-bitni)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
        - [CentOS 6.4,6.5,6.6 (samo 64-bitni)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
        - [Oracle Enterprise Linux 6.4,6.5 (samo 64-bitni)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
        - [SUSE Linux Enterprise Server SP3 (samo 64-bitni)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
        - Nakon ažuriranja poslužitelj za postupak ažuriranu verziju servisa mobilnost bit će dostupni u mapu C:\pushinstallsvc\repository na poslužitelju postupak.
    - **Mogućnost 2**: Ako imate računala pomoću starije verzije servisa mobilnost instaliran, možete automatski nadograditi usluga mobilnost na računalu s portala za upravljanje.

        1. Provjerite je li poslužitelj za postupak ažurira.
        2. Provjerite da zaštićeni strojno uspostavu [preduvjeti](#install-the-mobility-service-automatically) za automatski odaberite servis za mobilnost tako da je ažuriranje funkcionira ispravno.
        2. Odaberite grupu za zaštitu, isticanje zaštićeni računalu i kliknite **servis za ažuriranje mobilnost**. Ovaj gumb dostupna je samo ako je novija verzija servisa mobilnost. 

            ![Odaberite poslužitelj za vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

U odaberite računi navedite administratorski račun će se koristiti za ažuriranje servis mobilnost na zaštićenom poslužitelju. Kliknite u redu i pričekajte dovršetak okidačima posla.


## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>Korak 6: Dodavanje vCenter poslužitelja ili vSphere domaćini

1. Kliknite **poslužitelja** > **Konfiguraciju poslužitelja** > poslužitelj za konfiguraciju >**Dodaj vCenter Server** da biste dodali vCenter poslužitelja ili vSphere glavno računalo.

    ![Odaberite poslužitelj za vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)

2. Navedite detalje o poslužitelju ili glavno računalo, a zatim odaberite poslužitelj za postupak koji će se koristiti da biste ga otkriti.

    - Ako poslužitelj vCenter nije pokrenut na zadani priključak 443 navedite broj priključka koji je na kojemu je pokrenut vCenter poslužitelja.
    - Poslužitelj za postupak mora biti na istoj mreži kao glavnog poslužitelja/vSphere vCenter i trebali biste dobiti VMware vSphere EŽA 5.5.0 instaliran.

    ![postavke poslužitelja vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)


3. Po završetku otkrivanja poslužitelja vCenter će se prikazati u odjeljku Detalji o poslužitelju za konfiguraciju.

    ![postavke poslužitelja vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)

4. Ako koristite račun koji nisu administratori da biste dodali poslužitelja ili glavno računalo, provjerite je li račun ima ovlasti za sljedeće:

    - vCenter računi moraju imati podatkovnog centra, Datastore, mapu, glavno računalo, mreže, resursa, za pohranu prikaza, virtualnog računala i vSphere Distributed promjenu ovlasti omogućena.
    - računi za glavno računalo vSphere trebali imati podatkovnog centra, Datastore, mape, glavno računalo, mreže, resursa, virtualnog računala i vSphere Distributed promjenu ovlasti omogućeno



## <a name="step-7-create-a-protection-group"></a>Korak 7: Stvaranje grupe za zaštitu

1. Otvaranje **Stavki zaštićena** > **CORBIS** > **Stvaranje grupe za zaštitu**.

    ![Stvaranje grupe za zaštitu](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)

2. Na stranici s **Postavkama grupe za zaštitu odredite** Navedite naziv za grupu, a zatim odaberite poslužitelj za konfiguraciju na kojem želite stvoriti grupu.

    ![Zaštita od postavki grupe](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)

3. Na stranici **Postavke za određivanje replikacije** konfigurirati postavke replikacije koja će se koristiti za sve strojeva u grupi.

    ![Zaštita grupe replikacije](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)

4. Postavke:
    - **Višestruki VM dosljednost**: Ako to uključite stvara zajedničke aplikacije dosljedan oporavak upućuje na računalima u grupi zaštita. Ta je postavka najrelevantnije kada sve strojeva u grupi zaštita koriste isti radno opterećenje. Svim računalima će se oporaviti iste točke podataka. Dostupno je samo za poslužitelje sustava Windows.
    - **Prag RPO**: upozorenja će biti koji su generirani prilikom zaštite replikacije neprekinute podatke RPO premašuje konfigurirano RPO vrijednost praga.
    - **Oporavak pokažite zadržavanja**: određuje zadržavanja prozora. Zaštićeni strojeva se može oporaviti postavite u tom prozoru.
    - **Aplikacija dosljedan učestalost snimke**: određuje koliko se često oporavak točke koja sadrži aplikaciju dosljedan snimke stvorit će se.

Možete nadzirati grupi zaštita kao što su ste stvorili na stranici **Zaštićeni stavke** .

## <a name="step-8-set-up-machines-you-want-to-protect"></a>Korak 8: Postavljanje strojeva koji želite zaštititi

Morat ćete instalirati servis mobilnost na virtualnim računalima i fizičke poslužitelje koji želite zaštititi. To možete učiniti na dva načina:

- Automatski automatske i instalirajte servis na svakom računalu s poslužitelja postupak.
- Ručna instalacija servisa. 

### <a name="install-the-mobility-service-automatically"></a>Automatski instalira servis za mobilnost

Kada dodate strojeva grupi zaštita servis mobilnost se automatski pomiču i na svakom računalu instaliran poslužitelj za postupak. 

**Automatski automatske instalirajte servis mobilnost na poslužiteljima sustava Windows:** 

1. Instalirajte najnovija ažuriranja za poslužitelj za postupak opisan u [korak 5: instalirajte najnovija ažuriranja](#step-5-install-latest-updates), i provjerite je li poslužitelj za postupak dostupan. 
2. Provjerite je li postoji mrežne veze između računala izvora i poslužitelj za postupak i je li na računalu izvor može pristupiti s poslužiteljem postupak.  
3. Konfiguriranje Vatrozida za Windows da bi se **dijeljenje datoteka i pisača** i **WMI**. U odjeljku postavke vatrozida za Windows, odaberite mogućnost "Dopusti aplikaciju ili značajku u vatrozidu", a zatim odaberite aplikacije kao što je prikazano na slici u nastavku. Za strojeva koji pripadaju domene možete konfigurirati pravila vatrozida s GPO.

    ![Postavke vatrozida](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)

4. Račun koji se koristi za izvođenje instalacije automatske mora biti u grupe administratora na računalu na koje želite zaštititi. Za instalaciju automatske pružanja usluge za mobilnost samo koristi te vjerodajnice i ćete pošaljite im prilikom dodavanja stroj grupu zaštitu.
5. Ako navedeni račun nije račun domene morat ćete onemogućiti kontrolu za daljinski pristup korisnika na lokalnom računalu. Da biste učinili to dodajte stavku registra LocalAccountTokenFilterPolicy DWORD s vrijednost 1 u odjeljku HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. Da biste dodali unos registra iz otvorene cmd EŽA ili powershell i upišite **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. 

**Automatski automatske instalirajte servis mobilnost na poslužiteljima Linux:**

1. Instalirajte najnovija ažuriranja za poslužitelj za postupak opisan u [korak 5: instalirajte najnovija ažuriranja](#step-5-install-latest-updates), i provjerite je li poslužitelj za postupak dostupan.
2. Provjerite je li postoji mrežne veze između računala izvora i poslužitelj za postupak i je li na računalu izvor može pristupiti s poslužiteljem postupak.  
3. Provjerite je li račun korijenski korisnika na s izvorišnim poslužiteljem Linux.
4. Datoteka /etc/hosts na izvornom poslužitelju Linux provjerite sadrži li stavke koje IP adrese pridružene sve NIC-ovi mapiranje na lokalni naziv glavnog računala.
5. Instalirajte najnovije openssh, openssh server, openssl paketa na računalu na koje želite zaštititi.
6. Provjerite je li SSH i pokrenut na priključak 22. 
7. Omogući SFTP podsustav i lozinku za provjeru autentičnosti u datoteci sshd_config na sljedeći način: 

    - a) prijavite se kao korijen.
    - b) u datoteci /etc/ssh/sshd_config datoteke, pronađite redak koji započinje **PasswordAuthentication**.
    - c) uklonite redak i promijenite vrijednost "ne" u "da".

        ![Linux mobilnost](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)

    - d) pronađite redak koji započinje podsustav, a zatim uklonite redak.
    
        ![Linux automatske mobilnost](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    

8. Provjerite je li podržana varijante Linux izvornog računala. 
 
### <a name="install-the-mobility-service-manually"></a>Ručno instalirajte servis za mobilnost

Softverskih paketa koristili za instaliranje servisa mobilnost nalaze se na poslužitelj za postupak u C:\pushinstallsvc\repository. Prijavite se na poslužitelj za postupak i kopirajte odgovarajući instalacijski paket na računalo izvora koji se temelji na tablici u nastavku:-

| Izvorni operacijski sustav                               | Mobilnost paketa na poslužitelju postupak                                                            |
|---------------------------------------------------    |------------------------------------------------------------------------------------------------------ |
| Windows Server (samo 64-bitni)                          | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe`         |
| CentOS 6,4, 6.5, 6.6 (samo 64-bitni)                    | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz`     |
| SUSE Linux Enterprise Server 11 SP3 (samo 64-bitni)     | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz`|
| Oracle Enterprise Linux 6,4, 6.5 (samo 64-bitni)        | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz`       |


**Da biste instalirali servis mobilnost ručno na poslužitelju sustava Windows**, učinite sljedeće:

1. Kopirajte paketa **Microsoft ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** iz put direktorija poslužitelja postupak naveden u gornjoj tablici na računalo izvora.
2. Instalirajte servis za mobilnost tako da pokrenete izvršnu datoteku na računalu izvora.
3. Slijedite upute za instalacijski program.
4. Odaberite **servis za mobilnost** kao uloga, a zatim kliknite **Dalje**.
    
    ![Instalirajte servis za mobilnost](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)

5. Ostavite Instalacijski imenik kao zadani put instalacije, a zatim kliknite **Instaliraj**.
6. U **Config Agent glavno računalo** navedite IP adresa i HTTPS priključak poslužitelja za konfiguraciju.

    - Ako se povezujete putem Interneta kao priključak navesti javno virtualna IP adresa i javne HTTPS krajnjoj točki.
    - Ako se povezujete putem VPN odredite internu IP adresa i 443 za priključak. Napusti **Korištenje HTTPS** provjerena.

    ![Instalirajte servis za mobilnost](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)

7. Navedite pristupni izraz za konfiguraciju poslužitelja, a zatim kliknite **u redu** da biste registrirali servis mobilnost s poslužitelja za konfiguraciju.

**Da biste pokrenuli iz naredbenog retka:**

1. Kopiranje pristupni izraz iz na CX datoteku "C:\connection.passphrase" na poslužitelju i pokrenite sljedeću naredbu. U našem primjeru CX web-mjesto i 104.40.75.37 i u okvir za priključak HTTPS je 62519:

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**Instalirajte servis mobilnost ručno na poslužitelju Linux**:

1. Kopirajte u arhivu odgovarajuće tar koji se temelji na gornjoj tablici, s poslužitelja procesa s računalom izvora.
2. Otvorite program ljuske i izdvojiti zipane tar arhiva za lokalni put izvršavanjem`tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Stvaranje datoteke passphrase.txt u lokalnom direktoriju koji ste izdvojili sadržaj u arhivu tar tako da unesete *`echo <passphrase> >passphrase.txt`* iz ljuske.
4. Instalirajte servis za mobilnost unosom *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Navedite IP adresa i priključaka:

    - Ako se povezujete s poslužitelja za konfiguraciju putem Interneta navedite Konfiguracija virtualne javnu IP adresa poslužitelja i javne HTTPS krajnje točke u `<IP address>` i `<port>`.
    - Ako se povezujete putem veze za VPN odredite internu IP adresa i 443.

**Da biste pokrenuli iz naredbenog retka**:

1. Kopiranje pristupni izraz iz na CX datoteku "passphrase.txt" na poslužitelju i pokrenite ovaj naredbe. U našem primjeru CX web-mjesto i 104.40.75.37 i u okvir za priključak HTTPS je 62519:

Da biste instalirali na poslužitelju proizvodnje:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt
 
Da biste instalirali na poslužitelju ciljni:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

>[AZURE.NOTE] Kada dodate preskače strojeva grupi zaštita koji su već pokrenuti odgovarajuću verziju servisa mobilnost zatim automatske instalacije.


## <a name="step-9-enable-protection"></a>Korak 9: Zaštita Omogući

Da biste omogućili zaštitu virtualnim strojevima i dodate fizičke poslužitelji u grupu zaštitu. Prije nego što počnete, imajte na umu da:

- Virtualnim strojevima slučaju otkrivanja svakih 15 minuta, a može potrajati do 15 minuta da se prikazuju u oporavak Azure web-mjesta nakon otkrivanje.
- Okruženje za promjene na virtualnog računala (kao što je instalacija Alati VMware) može potrajati i do 15 minuta ažurirati u oporavak web-mjesta.
- Možete potražiti na zadnji put otkriven u polje **ZADNJEG kontakt pri** glavnog računala poslužitelja/ESXi vCenter na stranici **Konfiguracija poslužitelja** .
- Ako ste već stvorili grupu za zaštitu i dodavanje vCenter glavnog poslužitelja ili ESXi nakon toga traje petnaest minuta za portal za oporavak Azure web-mjesta da biste osvježili i virtualnim strojevima navoditi u dijaloškom okviru **Dodavanje strojeva grupi zaštita** .
- Ako želite odmah nastavite s dodavanjem strojeva grupi zaštita bez čekati zakazano otkrivanje, isticanje poslužitelj za konfiguraciju (nemojte kliknuti ga) i kliknite gumb **Osvježi** .
- Kada dodate virtualnim strojevima ili fizičke strojeva grupi Zaštita, poslužitelj za postupak automatski ih gura i instalira servis mobilnost na izvornom poslužitelju ako je to nije već instaliran.
- Za automatsko slanje mehanizam rad obavezno postavite na zaštićenom strojeva opisan u prethodnom koraku.

Dodajte strojeva na sljedeći način:

1. Kliknite **zaštićeni stavke** > **CORBIS** > **strojeva** > **Dodavanje računala**. Kao najbolji način preporučujemo zaštitu grupe treba odražavati vaše radnih opterećenja tako da dodate računala izvode određenu aplikaciju istoj grupi.
2. U **Odaberite virtualnim strojevima** ako ste zaštite fizičke poslužitelji, u čarobnjaku za **Dodavanje fizičke strojeva** navedite IP adresa i neslužbeni naziv. Zatim odaberite obitelji operacijski sustav.

    ![Dodavanje poslužitelja centar za V](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)

3. U **Odaberite virtualnim strojevima** ako ste zaštite VMware virtualnim strojevima, odaberite poslužitelj vCenter za upravljanje virtualnim strojevima (ili EXSi glavnog računala na kojem se izvodi), a zatim strojeva.

    ![Dodavanje poslužitelja centar za V](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png) 
4. **Određivanje ciljnog** resursa odaberite osnovne ciljnim poslužiteljima i prostora za pohranu koristite za replikaciju i odaberite hoće li se postavke želite koristiti za sve radnih opterećenja. Odaberite [Račun za pohranu Premium](../storage/storage-premium-storage.md) tijekom konfiguriranja zaštite radnih opterećenja koji zahtijevaju dosljedan visoke performanse IO i niske latencije da bi se hostira IO intenzivno radnih opterećenja. Ako želite koristiti za pohranu Premium račun za svoje radno opterećenje diskova trebate koristiti cilj matrica niza DS. Ne možete koristiti za pohranu Premium diskova s matrica cilj koji nisu DS-nizom.

    >[AZURE.NOTE] Ne podržavamo Premjesti račune za pohranu stvoren pomoću [Novi Azure portal](../storage/storage-create-storage-account.md) preko grupe resursa.

    ![vCenter poslužitelja](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)

5. U **Navedite računa** odaberite račun koji želite koristiti za instalaciju servisa mobilnost na zaštićenom strojeva. Vjerodajnice računa su vam potrebne za automatsku instalaciju mobilnost servisa. Ako ne možete odabrati račun Provjerite jeste li postavili jednu kao što je opisano u koraku 2. Imajte na umu da taj račun nije moguće pristupiti Azure. Za Windows server račun mora sadržavati administratorske ovlasti na izvornom poslužitelju. Linux račun mora biti korijen.

    ![Linux vjerodajnice](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)

6. Kliknite kvačicu da biste završili dodavanje strojeva grupi Zaštita i pokrenuli početne replikacije na svakom računalu. Možete nadzirati status na stranici **Zadaci** .

    ![Dodavanje poslužitelja centar za V](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)

7. Uz to je moguće nadzirati status zaštite tako da kliknete **Zaštićeni stavke** > Zaštita naziv grupe > **virtualnih računala** . Nakon početnog replikacije dovršava i strojeva sinkronizirate podataka će prikazati **zaštićeno** stanje.

    ![Zadaci virtualnog računala](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)


### <a name="set-protected-machine-properties"></a>Svojstva skupa zaštićeni računala

1. Kada na računalu sa stanjem **zaštićenog** možete konfigurirati svojstva prebacivanje. U zaštitu Detalji grupe odaberite računalo i otvorite karticu **Konfiguracija** .
2. Možete izmijeniti naziv koji će biti odobren na računalo u Azure nakon prebacivanje i veličine Azure virtualnog računala. Možete odabrati i Azure mrežu na kojoj će se povezati na računalu nakon prebacivanje.

    > [AZURE.NOTE] [Migracija mreže](../resource-group-move-resources.md) preko grupe resursa unutar iste pretplate ili putem pretplate nije podržana za mreže koji se koristi za implementaciju oporavak web-mjesta.

    ![Postavite svojstva virtualnog računala](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Imajte na umu da:

- Naziv Azure računalo mora biti usklađene sa Azure preduvjeti.
- Prema zadanim postavkama repliciranu virtualnim strojevima Azure niste povezani s Azure mreže. Ako želite da se repliciranu virtualnim strojevima komunikaciju obavezno postavljene na istoj Azure mreži za njih.
- Ako promijenite veličinu glasnoću na VMware virtualnog računala ili poslužitelj za fizičke prelazi u stanje od ključne važnosti. Ako vam je potrebna za promjenu veličine, učinite sljedeće:

    - a) promijenite postavku veličina.
    - b) na kartici **virtualnim strojevima** odaberite virtualnog računala, a zatim kliknite **Ukloni**.
    - c) u **Uklanjanje virtualnog računala** odaberite mogućnost **Onemogući zaštitu (koristite za oporavak dubinsku analizu i glasnoću promjenu veličine)**. Ta mogućnost onemogućuje zaštitu, ali se zadržavaju oporavak točke u Azure.

        ![Postavite svojstva virtualnog računala](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)

    - d) ponovno omogućili zaštitu virtualnog računala. Kada ponovno omogućili zaštite podataka promijenjene veličine jedinice će se prenijeti Azure.

    

## <a name="step-10-run-a-failover"></a>10 korak: Pokretanje programa prebacivanje

Trenutno može pokrenuti samo neplanirano failovers zaštićene VMware virtualnim strojevima i fizičke poslužiteljima. Imajte na umu sljedeće:



- Prije nego što započnete s pomoćnim ciljnim poslužiteljima konfiguraciju i matrica provjerite jesu li pokrenuti i dokumenata. U suprotnom neće uspjeti prebacivanje.
- Izvor strojeva ne isključite kao dio neplanirano prebacivanje. Izvođenje neplanirano prebacivanje zaustavlja replikacije podataka za zaštićeni poslužitelje. Morat ćete izbrisati strojeva iz grupe zaštitu i dodajte ih ponovno da biste pokrenuli ponovno Zaštita računala nakon dovršetka neplanirano prebacivanje.
- Ako želite neće uspjeti putem ne izgubite podatke, provjerite je li virtualnim strojevima primarni web-mjesta isključeni su prije nego što započnete s pomoćnim.

1. Na **Tarife za oporavak** stranice, a zatim dodajte plan za oporavak. Navedite detalje za plan, a zatim odaberite **Azure** kao cilj.

    ![Konfiguriranje oporavak plan](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)

2. U **Odaberite virtualnog računala** odaberite grupu za zaštitu, a zatim odaberite strojeva u grupu koju želite dodati tarifu za oporavak. [Dodatne informacije potražite u](site-recovery-create-recovery-plans.md) o tarifama za oporavak.

    ![Dodavanje virtualnim strojevima](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)

3. Neuspješne potrebno možete prilagoditi plan za stvaranje grupe i slijed redoslijed u kojim računalima oporavka plan su iznad. Možete dodati i upute za ručno akcije i skripti. Skripte kada oporavak Azure možete dodati pomoću [Runbooks Automatizacija Azure](site-recovery-runbook-automation.md).

4. Na stranici **Oporavak tarife** odaberite tarifu, a zatim kliknite **Neplanirano prebacivanje**.
5. U **Potvrdi prebacivanje** provjerite je li smjer prebacivanje (za Azure), a zatim odaberite oporavak točke uvoza iznad da biste.
6. Pričekajte prebacivanje zadatak da biste dovršili i zatim provjerite je li u prebacivanje uspješnog prema očekivanjima i uspješno pokretanje repliciranu virtualnim strojevima u Azure.




## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>Korak 11: Nije uspjelo ponovno nije uspjela putem računala s Azure

[Saznajte više](site-recovery-failback-azure-to-vmware-classic-legacy.md) o tome da bi se prikazala sustava nije uspjelo putem računala izvode Azure sigurnosno lokalnog okruženja.


## <a name="manage-your-process-servers"></a>Upravljanje poslužitelja postupak

Poslužitelj za postupak šalje replikacije podatke na glavni cilj server Azure i otkrije novi VMware virtualnim strojevima dodali vCenter poslužitelj. U sljedećim slučajevima možda ćete morati promijeniti poslužitelj za postupak u implementaciji sustava:

- Ako je trenutni poslužitelj za postupak funkcionira
- Ako je vaš cilj oporavak zareza (RPO) dolazi prihvatljiva razine za tvrtku ili ustanovu.

Po potrebi možete premjestiti replikacije neke ili sve lokalne VMware virtualnim strojevima i fizičke poslužitelje na poslužitelju drugi proces. Ako, na primjer:

- **Nije uspjelo**– ako poslužitelj za postupak ne uspije ili nije dostupna zaštićeni strojno replikacije možete premjestiti na drugi poslužitelj za postupak. Metapodaci izvornog računala i replike strojnog će se premjestiti u novi poslužitelj za postupak, a zatim je resynchronized podataka. Novi poslužitelj za postupak automatski će se povezati s poslužiteljem vCenter za automatsko otkrivanje. Možete nadzirati stanje postupak poslužitelja na nadzornoj ploči za oporavak web-mjesta.
- **Da biste prilagodili RPO za ujednačavanje opterećenja**– poboljšane vam za ujednačavanje opterećenja odaberite poslužitelj za drugi proces na portalu za oporavak web-mjesta te premjestili replikacije jedan ili više računala ga ručno opterećenja. U ovom slučaju metapodataka od odabranog izvora i replike strojeva se premješta u novi poslužitelj za postupak. Izvorni poslužitelj za postupak ostati povezani s poslužiteljem vCenter. 

### <a name="monitor-the-process-server"></a>Praćenje poslužitelj za postupak

Ako je poslužitelj za postupak u Kritično stanje na nadzornoj ploči za oporavak web-mjesta prikazat će se upozorenje o stanju. Možete kliknuti status da biste otvorili karticu događaja i zatim Dubinska analiza prema dolje određene poslove na kartici zadaci. 

### <a name="modify-the-process-server-used-for-replication"></a>Izmjena postupak poslužitelja koji se koristi za replikaciju

1. Otvorite **poslužitelje** > **Konfiguraciju poslužitelja** > poslužitelj za konfiguraciju > **Detalji o poslužitelju**.
2. Kliknite **Postupak poslužitelja** > **Poslužitelj za postupak promjene** pokraj poslužitelja koji želite izmijeniti.

    ![Promjena poslužitelja postupak 1](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)

3. **Promjena postupak**Server > **Ciljani postupak poslužitelj** odaberite novi poslužitelj koji želite koristiti, a zatim odaberite virtualnim strojevima koje želite replicirati na poslužitelj za nove. Kliknite ikonu informacije pokraj naziva poslužitelja za pojedinosti slobodnog prostora i korištene memorije. Prosječna prostora koji će se tražiti da replicirati svaki odabrani virtualnog računala da biste novi poslužitelj za postupak prikazuje se da bi vam učitavanje odluka.

    ![Promjena poslužitelja postupak 2](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)

4. Kliknite kvačicu da biste započeli replikaciju novi poslužitelj za postupak. Imajte na umu da Ako uklonite sve virtualnim strojevima s poslužitelja za postupak koji je ključnih je potrebno više neće prikazivati ključnih upozorenje na nadzornoj ploči.


## <a name="third-party-software-notices-and-information"></a>Obavijesti trećih strana softver i podataka

Ne šalji prijevod i Localize

Softver i opremu koji se izvodi u Microsoft proizvoda ili usluga temelji se na ili ugrađuje materijala iz projekata navedene (skupno, "trećih strana kod").  Microsoft se neće izvorni autor šifre drugih proizvođača.  Izvorni autorskim i licence, u odjeljku Microsoft primili takve kod drugih proizvođača postavljene su naprijed ispod.

Informacije u odjeljku A koji se odnosi kod drugih proizvođača komponente s projektima navedena u nastavku. Informativna svrhe su dani takve licence i informacije.  Kod drugih proizvođača je u tijeku relicensed vam Microsoft u odjeljku o licenciranju odredbe za Microsoftov softver Microsoftovih proizvoda ili usluge.  

Informacije u odjeljku B koji se odnosi komponente kod drugih proizvođača koji su se bili dostupni na Microsoft u odjeljku izvorni uvjete licenciranja.

Cijeli dokument nalazi se u [Microsoftovu centru za preuzimanje](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft zadržava sva prava izričito ne odobren spominju u ovom dokumentu, po patentnim, estoppel ili neki drugi način.
