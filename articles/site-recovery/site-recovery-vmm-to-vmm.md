<properties
    pageTitle="Hyper-V virtualnim strojevima u VMM oblaka replikaciju sekundarnu VMM web-mjestu pomoću portala za Azure | Microsoft Azure"
    description="U članku se opisuje kako implementirati Azure oporavak web-mjesta da biste orkestrirali replikacije, prebacivanje i oporavak Hyper-V VMs u VMM oblaka na sekundarnom VMM web-mjesto pomoću portala za Azure."
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
    ms.date="08/23/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-the-azure-portal"></a>Hyper-V virtualnim strojevima u VMM oblaka replikaciju sekundarnu VMM web-mjestu pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal za Azure](site-recovery-vmm-to-vmm.md)
- [Klasični portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell – Voditelj resursa](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Dobro došli u oporavak Azure web-mjesta! Koristite ovaj članak ako želite replicirati lokalnog Hyper-V virtualnim strojevima upravlja se iz oblaka sustava centra virtualnog računala Manager (VMM) da biste sekundarnog web-mjesta. U ovom se članku opisuje kako postaviti replikacije pomoću oporavak Azure web-mjesta na portalu za Azure.

> [AZURE.NOTE] Azure sadrži dvije različite [implementacije modela](../resource-manager-deployment-model.md) za stvaranje i rad s resursima: Voditelj resursa Azure i classic. Azure ima dvije portalima – Azure klasični portala koji podržava model klasični implementacije i portalu Azure s podrškom za oba modele implementacije.


Azure oporavak web-mjesta na portalu za Azure nudi nekoliko novih značajki:

- U na Azure portal, sigurnosno kopiranje Azure i oporavak Azure web-mjesta servisa kombiniraju se u jednu oporavak servisa sigurnog, tako da možete postaviti i upravljanje poslovanje i Izrada oporavak (BCDR) s jednog mjesta. Nadzorna ploča za Sjedinjeno komuniciranje omogućuje nadzor i upravljanje operacije na web-mjesta na lokalni Azure javno poslužitelja u oblak i.
- Korisnici s Azure pretplate dodjeli programom oblaka rješenje davatelj (CSP) sada mogu upravljati operacije oporavak web-mjesta na portalu za Azure.


Kad pročitate članak, pri dnu u komentarima Disqus objavljuju komentare. Zamolite tehnička pitanja na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Pregled

Tvrtke i ustanove moraju strategije BCDR koja određuje kako aplikacija, radnih opterećenja i podataka ostati pokrenut i dostupne tijekom planirane i Neplanirana isključiti, a čim oporaviti normalno funkcionira uvjeta. Strategije BCDR trebali biste zadržali poslovnih podataka sigurnih i koje se mogu vratiti i provjerite je li radnih opterećenja ostaju stalno dostupne kada se pojavi Izrada.

Oporavak web-mjesta je Azure service doprinosa strategije BCDR po orchestrating replikacije lokalne poslužitelje fizičke i virtualnih računala s oblakom (Azure) ili sekundarni podatkovnog centra. Prilikom kvarove u svom primarnom mjestu, koji se neće putem sekundarne mjesto da bi aplikacija i radnih opterećenja dostupna. Ne natrag na svom primarnom mjestu kada on se vraća na običnom operacije. Dodatne informacije u [oporavak web-mjesta Azure?](site-recovery-overview.md)

Ovaj članak sadrži sve potrebne informacije za replikaciju lokalnog Hyper-V VMs u VMM oblaka sekundarne VMM web-mjesto. Uključuje se pregled arhitekture, Planiranje informacije i koraci za implementaciju za konfiguriranje lokalne poslužitelje, postavke replikacije i planiranje kapaciteta. Nakon što ste postavili Infrastruktura možete omogućiti replikacije na koje želite zaštititi, a zatim potvrdite taj pomoćni funkcionira.

## <a name="business-advantages"></a>Prednosti tvrtke

- Oporavak web-mjesta omogućuje zaštitu radnih opterećenja tvrtke i aplikacije koje rade na VMs Hyper-V tako da ih replikaciju sekundarnu Hyper-V poslužitelj.
- Portal za oporavak servisa predstavlja jedno mjesto za postavljanje, upravljanje i praćenje replikacije, prebacivanje i oporavak.
- Jednostavno izvođenja failovers možete pokrenuti iz infrastruktura za primarni lokalnog sekundarnog web-mjesta i failback (Vraćanje) s web-mjesta sekundarne primarni.
- Tarife za oporavak s više računala možete konfigurirati tako da se tiered aplikacije radnih opterećenja neće uspjeti putem zajedno.

## <a name="scenario-architecture"></a>Scenarij arhitekture

Ovo su komponente scenarija:

- **Primarni web-mjesta**: primarni web-mjestu postoji jedan ili više Hyper-V glavnog računala poslužitelja izvodi izvora VMs želite replicirati. Sljedećim poslužiteljima primarni glavno računalo nalaze se u VMM oblaka za privatno.
- **Sekundarni web-mjesta**: U sekundarnog web-mjesta su jedan ili više Hyper-V glavno računalo poslužitelji izvodi cilj VMs na koji je replicirati primarni VMs. Sljedećim poslužiteljima glavno računalo nalaze se u VMM oblaka za privatno. Oblak može biti na primarnom poslužitelju (Ako imate samo jedan VMM poslužitelja) ili sekundarni poslužitelj VMM.
- **Davatelj usluga**: tijekom uvođenja oporavak web-mjesta, instalirajte davatelja za oporavak Azure web-mjesta na poslužiteljima VMM, a registrirali ti poslužitelji oporavak servisa sigurnog. Davatelj usluga na poslužitelju VMM putem HTTP 443 za replikaciju djelovanje komunicira s oporavak web-mjesta. Replikacija podataka pojavljuje se između primarnih i sekundarnih poslužitelja Hyper-V glavnog računala. Repliciranu podataka ostaje unutar lokalnog web-mjesta i mreža i ne šalje Azure. Dodatne informacije o [privatnosti](#privacy-information-for-site-recovery).

![Topologija E2E](./media/site-recovery-vmm-to-vmm/architecture.png)

### <a name="data-privacy-overview"></a>Pregled podataka o zaštiti privatnosti

U ovoj su tablici ukratko se prikazuju kako se podaci se pohranjuju u ovom scenariju:
****
Akcija | **Pojedinosti** | **Prikupljene podatke** | **Korištenje** | **Obavezno**
--- | --- | --- | --- | ---
**Registracija** | Oporavak servisa sigurnog registrirajte VMM poslužitelja. Ako kasnije želite poništiti registraciju na poslužitelju, to možete učiniti tako da izbrišete informacije o poslužitelju s portala za Azure. | Kada je registrirana na poslužitelju VMM oporavak web-mjesta prikuplja procesi i prenosi metapodataka o VMM poslužitelja i nazive oblaka VMM otkrio oporavak web-mjesta. | Podaci se koristi za prepoznavanje i komunikaciju na odgovarajući poslužitelj za VMM i konfiguriranje postavki za odgovarajući VMM oblaka. | Značajka je potreban. Ako ne želite poslati te podatke za oporavak web-mjesta bi trebalo koristiti servis za oporavak web-mjesta.
**Omogućivanje replikacije** | Oporavak davatelja za Azure web-mjesta je instalirana na poslužitelju VMM i je conduit za komunikaciju sa servisom za oporavak web-mjesta. Davatelj je biblioteka dinamičkih veza (DLL) nalazi u postupku VMM. Nakon instalacije davatelj značajka "Podatkovnim centrom oporavak" dobiva omogućen u administratorskoj konzoli VMM. Nove i postojeće VMs možete omogućiti tu značajku omogućuje zaštitu na VM. | Sa skupom ovo svojstvo davatelj šalje ime i ID-JA na VM oporavak web-mjesta.  Replikacija omogućena je prema Windows Server 2012 ili Windows Server 2012 R2 Hyper-V replike. Dohvaća podatke virtualnog računala replicirati s jednog Hyper-V glavnog računala na drugo (obično nalazi u podatkovnom centru različite "oporavak"). | Oporavak web-mjesta koristi metapodataka za popunjavanje VM informacije na portalu za Azure. | Ta je značajka ključna dio usluge i ne može se isključiti. Ako ne želite poslati te podatke, ne omogućite zaštiti oporavak web-mjesta za VMs. Imajte na umu da se putem HTTP šalju svih podataka koji se šalju davatelj za oporavak web-mjesta.
**Oporavak plan** | Tarife za oporavak vam sastavljanje djelovanje Osmišljavanje podatkovnog centra za oporavak. Možete definirati redoslijed kojim VMs ili grupe virtualnim strojevima počne na web-mjestu za oporavak. Možete odrediti i sve automatiziranog skripte se izvodi ili sve ručne akcije da bi se otvorila, u trenutku oporavak za svaki VM. Prebacivanje obično se pokreće na razini plan oporavak usklađenih oporavka. | Oporavak web-mjesta prikuplja, obrađuje i prenosi metapodataka za tarifu za oporavak, uključujući metapodataka virtualnog računala i metapodaci sve Automatizacija skripte i ručno akcija bilješke. | Metapodaci koristi se za stvaranje plana oporavak na portalu za Azure. | Ta je značajka ključna dio usluge i ne može se isključiti. Ako ne želite poslati te podatke za oporavak web-mjesta, ne stvorite tarife za oporavak.
**Preslikavanje** | Mrežni podatke iz centra za primarni podataka podatkovnog centra za oporavak. Kada su VMs oporaviti oporavak web-mjesta, preslikavanje pomaže u uspostavljanje veza s mrežom. | Oporavak web-mjesta prikuplja, obrađuje i prenosi metapodataka logičke mreža za svakog web-mjesta (primarni i podatkovnim centrom). | Metapodaci koristi se za ispunjavanje mrežne postavke tako da možete mapirati podatke o mreži. | Ta je značajka ključna dio usluge i ne može se isključiti. Ako ne želite poslati te podatke za oporavak web-mjesta, nemojte koristiti preslikavanje.
**Prebacivanje (planirano i Neplanirana/testiranje)** | Prebacivanje neće uspjeti putem VMs iz centra za jednu VMM upravljanih podataka u drugu. Prebacivanje akciju se pokreće ručno, a na portalu za Azure. | Davatelj usluga na poslužitelju VMM oporavak web-mjesta obavijestiti o događaju prebacivanje i izvodi akciju za prebacivanje na glavnom računalu Hyper-V putem sučelja VMM. Stvarni prebacivanje na VM je od glavnih računala Hyper-V na drugu i obrađene Windows Server 2012 ili Windows Server 2012 R2 Hyper-V replike. Po dovršetku prebacivanje davatelja usluge na poslužitelju VMM u centru za oporavak podataka šalje informacije za uspjeh oporavak web-mjesta. | Oporavak web-mjesta koristi podaci koji se šalju za popunjavanje status podataka akcija za prebacivanje na portalu za Azure. | Ta je značajka ključna dio usluge i ne može se isključiti. Ako ne želite poslati te podatke za oporavak web-mjesta, nemojte koristiti prebacivanje.


## <a name="azure-prerequisites"></a>Preduvjeti za Azure

Evo što vam je potrebno u Azure za implementaciju scenarij:

**Preduvjeti** | **Pojedinosti**
--- | ---
**Azure**| Potreban račun [Sustava Microsoft Azure](http://azure.microsoft.com/) . Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). [Dodatne informacije](https://azure.microsoft.com/pricing/details/site-recovery/) o cijenama oporavak web-mjesta.


## <a name="on-premises-prerequisites"></a>Lokalni preduvjeti

Evo što vam je potrebno primarnih i sekundarnih lokalnog web-mjesta za implementaciju scenarij:

**Preduvjeti** | **Pojedinosti**
--- | ---
**VMM** | Preporučujemo da implementacija poslužitelja VMM u primarni web-mjesta i VMM poslužitelja u sekundarnog web-mjesta.<br/><br/> Možete i [replicirati između oblaka na jednom VMM poslužitelju](site-recovery-single-vmm.md). Da biste to učinili morate barem dva oblaka konfiguriran na poslužitelju VMM.<br/><br/> VMM poslužitelja mora imati najmanje 2012 SP1 centar sustava s najnovijim ažuriranjima.<br/><br/> Svaki VMM poslužitelja mora biti na jednu ili više oblaka konfigurirati i sve oblaka moraju imati profil Hyper-V kapaciteta postavljen. <br/><br/>Oblaka mora sadržavati jednu ili više grupa VMM glavnog računala.<br/><br/>Dodatne informacije o postavljanju oblaka VMM u [konfiguraciji oblaka tkanina VMM](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), a [vodič: Stvaranje privatne oblaka pomoću sustava centra 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> Poslužitelji VMM potreban pristup Internetu.
**Hyper-V** | Hyper-V poslužitelje morate imati barem Windows Server 2012 s ulogom Hyper-V i instalirali najnovija ažuriranja.<br/><br/> Hyper-V poslužitelja smiju sadržavati jedan ili više VMs.<br/><br/>  Glavno računalo Hyper-V poslužitelji se nalaze na glavno računalo grupe u VMM oblaka primarnih i sekundarnih.<br/><br/> Ako koristite Hyper-V klasteru u sustavu Windows Server 2012 R2 instalirajte [Ažuriranje 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Ako koristite Hyper-V klasteru u sustavu Windows Server 2012, imajte na umu te broker klaster nije automatski stvara ako imate statičke IP adresa sustavom klaster. Ćete morati ručno konfigurirati broker klaster. [Dodatne informacije potražite u](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Davatelj usluga** | Tijekom uvođenja oporavak web-mjesta, instalirajte davatelja za oporavak Azure web-mjesta na poslužiteljima VMM. Davatelj putem HTTP 443 da biste orkestrirali replikacije komunicira s oporavak web-mjesta. Replikacija podataka pojavljuje se između Hyper-V poslužitelja primarnih i sekundarnih putem LAN-a ili VPN veza.<br/><br/> Davatelj usluga na poslužitelju VMM treba pristup URL-ove: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.Windows.NET; *. backup.windowsazure.com; *. blob.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Uz to dopustiti komunikaciju vatrozid s poslužitelja VMM [Azure podatkovnog centra IP rasponi](https://www.microsoft.com/download/confirmation.aspx?id=41653) i omogućuju protokol HTTPS (443).

## <a name="prepare-for-deployment"></a>Priprema za implementaciju

Priprema za implementaciju morate:

1. [Priprema VMM poslužitelja](#prepare-the-vmm-server) za implementaciju oporavak web-mjesta.
2. [Priprema za preslikavanje](#prepare-for-network-mapping). Postavljanje mreže tako da možete konfigurirati preslikavanje.

### <a name="prepare-the-vmm-server"></a>Priprema VMM poslužitelja

Provjerite je li poslužitelj VMM uspostavu [preduvjeti](#on-premises-prerequisites) i možete pristupiti na popisu URL-ove.


### <a name="prepare-for-network-mapping"></a>Priprema za preslikavanje

Mrežni karte mapiranja između VMM VM mreže na poslužiteljima VMM primarnih i sekundarnih da biste:

- Optimalnog postavite replike VMs na sekundarnom Hyper-V domaćini nakon prebacivanje.
- Povezivanje replike VMs odgovarajuće VM mrežama.
- Ako ne konfigurirate mreže mapiranja replike VMs neće biti povezani s mreže nakon prebacivanje.
- Ako želite postaviti mrežu mapiranja tijekom oporavak web-mjesta implementacije ovdje je što vam je potrebno:

    - Provjerite je li VMs na izvornom poslužitelju Hyper-V glavno računalo povezani s mrežom VMM VM. Tu mrežu mora biti povezan s logičke mreže koji je pridružen oblaka.
    - Provjerite postoji li sekundarne oblaka koju koristite za oporavak odgovarajuće VM mreža konfigurirana. Tu mrežu VM mora biti povezan s logičke mreže povezane sa sekundarnom oblaka.

- [Dodatne informacije](site-recovery-network-mapping.md) o načinu funkcioniranja a preslikavanje.

## <a name="prepare-for-deployment-with-a-single-vmm-server"></a>Priprema za implementaciju s jednom VMM poslužitelju

Ako imate samo jedan VMM poslužitelja možete replicirati VMs u Hyper-V domaćini u oblaku VMM [Azure](site-recovery-vmm-to-azure.md) ili sekundarni VMM oblaka. Preporučujemo da prvu mogućnost jer nije objedinjenog replikaciju između oblaka, no ako vam je potrebna radnja ovdje je što vam je potrebno učiniti:

1. **Postavljanje VMM na Hyper-V VM**. Kada to predlažemo da colocate koristi VMM na istom VM instancu sustava SQL Server. To štedi vrijeme samo jedan VM ima će biti stvoren. Ako želite koristiti udaljene instancu sustava SQL Server i do prekida pojavljuje, morate vratiti tu instancu prije nego što se može oporaviti VMM.
2. **Provjerite je li poslužitelj VMM ima najmanje dva oblaka konfiguriran**. Jedan oblaka sadržavat će na VMs želite replicirati i u oblak će vam poslužiti kao sekundarni mjesto. Oblak koji sadrži VMs koji želite zaštititi moraju biti usklađene sa [preduvjeti](#on-premises-prerequisites).
3. Postavljanje oporavak web-mjesta kao što je opisano u ovom članku. Stvaranje i registraciju VMM poslužitelja u na sigurnog, postaviti pravilo replikacije i omogućiti replikaciju. Navedite da Početna replikacija odvija putem mreže.
4. Kada postavite preslikavanje mapirati na mreži VM primarni oblaka mrežu VM za sekundarne spremanja.
5. Na konzoli za Upravitelj Hyper-V Omogućivanje Hyper-V replike na glavnom računalu Hyper-V koja sadrži VMM VM i omogućiti replikaciju na na VM. Provjerite ne dodajete virtualnog računala VMM oblaka zaštićene oporavak web-mjesta da biste bili sigurni Hyper-V replike postavke nisu nadjačati oporavak web-mjesta.
6. Ako stvorite oporavak tarife za prebacivanje na isti poslužitelj VMM koji koristite za izvorišno i odredišno.
7. Neuspjeh iznad i vratiti na sljedeći način:

    - Ručno uspjeti putem VM VMM sekundarne web-mjesto pomoću upravitelja Hyper-V planiranog prebacivanje.
    - Pokazivač na VMs neće uspjeti.
    - Nakon -> primarni VMM VM sadrži je oporaviti, prijavite se na portal za Azure oporavak servisa sigurnog i pokrenuti neplanirano prebacivanje od na VMs s sekundarnog web-mjesta u primarni web-mjesta.
    - Po dovršetku neplanirano prebacivanje svih resursa možete pristupiti iz primarnog web-mjesta ponovno.


### <a name="create-a-recovery-services-vault"></a>Stvaranje oporavak servisa sigurnog

1. Prijavite se na [portal za Azure](https://portal.azure.com).
2. Kliknite **Novi** > **upravljanja** > **servise za oporavak**. Umjesto toga možete kliknuti **Pregledaj** > sefovi**Oporavak usluge** > **Dodaj**.

    ![Nove zbirke ključeva](./media/site-recovery-vmm-to-vmm/new-vault3.png)

3. U odjeljku **naziv** navedite neslužbeni naziv da biste odredili na zbirke ključeva. Ako imate više pretplata, odaberite jedan od njih.
4. [Stvori novu grupu resursa](../resource-group-template-deploy-portal.md) ili odaberite postojeći pa navedite Azure regiju. Strojeva će je replicirati na tom području. Da biste provjerili podržanih regija pogledajte geografske dostupnost u [Azure web-mjesta oporavak cijene detalja](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Ako želite brzo pristupiti na sigurnog na nadzornoj ploči kliknite **Prikvači na nadzornoj ploči** > **Stvaranje sigurnog**.

    ![Nove zbirke ključeva](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

Nove zbirke ključeva pojavit će se na **nadzornoj ploči** > **sve resurse**i na glavnom plohu **sefovi servise za oporavak** .




## <a name="getting-started"></a>Početak rada

Oporavak web-mjesta sadrži prvi koraci sučelje koji olakšava brzo moguće uvesti. Početak rada provjerava preduvjeti web-mjesta i vodi vas kroz implementacije koraka u redoslijedu desnom oporavak web-mjesta.

U prvi koraci odaberite vrstu strojeva želite za replikaciju i mjesto na koje želite replicirati na. Postavljanje lokalne poslužitelje, stvoriti pravila za replikaciju i izvodili planiranje kapaciteta. Nakon što ste postavili preduvjete infrastrukture Omogući replikacije za VMs. Pokrenite failovers za određene strojeva ili stvaranje tarife za oporavak uvoza na više računala.

Početak rada započeti tako da odaberete način na koji želite uvesti oporavak web-mjesta. Početak rada tijek promjene malo po vlastitom nahođenju replikacije.

## <a name="step-1-choose-your-protection-goal"></a>Korak 1: Odaberite Zaštita cilj

Odaberite što želite replicirati i mjesto na koje želite replicirati na.

1. U plohu **sefovi oporavak servisi** odaberite vaše zbirke ključeva, a zatim **Postavke**.
2. U odjeljku **Postavke** > **Početak rada** kliknite **Oporavak web-mjesta** > **Korak 1: Priprema infrastrukture** > **zaštitu cilj**.
3. U **cilju zaštite** odaberite **oporavak web-mjesta**pa odaberite **Da, pomoću značajke Hyper-V**.
4. Odaberite **da** da biste naznačili koristite VMM i upravljanje njima domaćini Hyper-V tako **da** potvrdite ako imate sekundarni poslužitelj VMM. Ako ste implementacija replikacije između oblaka na jednom poslužitelju VMM kliknite **nema**. Zatim kliknite **u redu**.

    ![Odaberite ciljeva](./media/site-recovery-vmm-to-vmm/choose-goals.png)


## <a name="step-2-set-up-the-source-environment"></a>Korak 2: Postavljanje okruženja izvora

Instalirajte davatelja za oporavak Azure web-mjesta na poslužiteljima VMM, a registrirali poslužitelji u zbirke ključeva.

1. Kliknite **Korak 2: Priprema infrastrukture** > **izvora**.

    ![Postavljanje izvora](./media/site-recovery-vmm-to-vmm/goals-source.png)

2. **Priprema izvorne** kliknite **+ VMM** da biste dodali VMM poslužitelja.

    ![Postavljanje izvora](./media/site-recovery-vmm-to-vmm/set-source1.png)

2. U provjeri plohu **Dodavanje poslužitelja** koji će se prikazati **VMM centar sustava poslužitelja** u **vrsti poslužitelj** i koje poslužitelj VMM zadovoljava na [preduvjete i preduvjete URL-a](#on-premises-prerequisites).
4. Preuzmite instalacijsku datoteku davatelja za oporavak Azure web-mjesta.
5. Preuzmite ključa za registraciju. Morate to pri pokretanju instalacijskog programa. Ključno je valjan 5 dana nakon ga stvoriti.

    ![Postavljanje izvora](./media/site-recovery-vmm-to-vmm/set-source3.png)

6. Instalirajte davatelja za oporavak Azure web-mjesta na poslužitelju VMM.

> [AZURE.NOTE] Ne morate izričito instalirati ništa na poslužiteljima Hyper-V glavnog računala.


### <a name="set-up-the-azure-site-recovery-provider"></a>Postavljanje davatelja za oporavak Azure web-mjesta

1. Pokrenite davatelj instalacijsku datoteku na svakom VMM poslužitelju. Ako je VMM implementiran klasteru i instalirate davatelj usluga za prvo ga instalirati na aktivni čvor i dovršetak instalacije da biste registrirali VMM poslužitelja u sigurnog. Zatim instalirajte davatelja usluge na ostale čvorove. Čvorovi klaster sve izvoditi istu verziju davatelja usluga.
2. Postavljanje izvodi nekoliko prerequirement provjere i zahtijeva dozvolu za zaustavljanje servisa VMM. Servis VMM će se ponovno pokrenuti automatski kada se dovrši instalacija. Ako ga instalirate na VMM klaster vas će se zatražiti da biste prestali klaster ulogu.

2.  U **Programu Microsoft Update** možete odabrati u ažuriranja tako da se instalacije ažuriranja davatelja skladu Pravilnik za Microsoft Update.
3. U **instalaciji** Prihvati ili izmjena zadano mjesto za instalaciju davatelja usluge, a zatim kliknite **Instaliraj**.

    ![Mjesto instalacije](./media/site-recovery-vmm-to-vmm/provider-location.png)

3. Nakon dovršetka instalacije kliknite **registrirati** da biste registrirali poslužitelja u zbirke ključeva.

    ![Mjesto instalacije](./media/site-recovery-vmm-to-vmm/provider-register.png)

9. U odjeljak **naziv sigurnog**provjerite naziv zbirke ključeva u kojem će biti registriran na poslužitelj. Kliknite *Dalje*.

    ![Registracija poslužitelja](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. U **Internetsku vezu** Navedite način na koji se davatelja usluge na poslužitelju VMM povezuje s Internetom. Odaberite **Poveži s postojeće postavke proxy poslužitelja** za korištenje na zadane postavke internetske veze konfiguriran na poslužitelju.

    ![Postavke internetske](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

    - Ako želite koristiti prilagođenu proxy ga trebali postaviti prije instalacije davatelja usluga. Kada konfigurirate postavke proxyja prilagođene test će se pokrenuti da biste provjerili proxy vezu.
    - Ako koristite prilagođeni proxy ili zadani proxy zahtijeva provjeru autentičnosti morate unijeti detalje proxy poslužitelj, uključujući adresu proxy poslužitelja i priključka.
    - Sljedeći URL-ovi trebali biste moći pristupiti poslužitelju VMM i domaćini Hyper-v
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Dopusti IP adrese opisane u [Azure podatkovnog centra IP rasponi](https://www.microsoft.com/download/confirmation.aspx?id=41653) i HTTPS protokola (443). Promijenile bijeli popis IP rasponi Azure regije koje namjeravate koristi i koje Zapad SAD-a.
    - Ako koristite prilagođeni proxy VMM RunAs računa (DRAProxyAccount) stvorit će se automatski pomoću vjerodajnica za navedeni proxy poslužitelj. Proxy poslužitelj konfigurirati tako da se ovaj račun možete provjeriti autentičnost uspješno. Postavke računa VMM RunAs moguće je izmijeniti na konzoli VMM. Da biste to učinili, otvoriti radni prostor **Postavke** , proširite **Sigurnost**, kliknite **Pokreni kao računi**, a zatim promijenite lozinku za DRAProxyAccount. Morat ćete ponovno pokrenite servis VMM tako da se ova postavka stupa na snagu.


8. U **Ključa za registraciju**odaberite ključ koji se preuzimaju s oporavak Azure web-mjesta i kopirane na VMM poslužitelj.


10.  Postavka šifriranja koristi samo kada ste replikaciju Hyper-V VMs u VMM oblaka za Azure. Ako ste replikaciju sekundarnu web-mjesto se ne koristi.

11.  U odjeljak **naziv poslužitelja**navedite neslužbeni naziv za prepoznavanje VMM server u zbirke ključeva. U konfiguraciji klaster Navedite naziv uloge VMM klaster.
12.  Odaberite **Sinkroniziraj oblaka metapodataka** želite li da biste sinkronizirali metapodataka za sve oblaka na poslužitelju VMM s na zbirke ključeva. Ova akcija samo se mora dogoditi kada na svakom poslužitelju. Ako ne želite da biste sinkronizirali sve oblaka, ostavite tu postavku poništen, a sinkronizirati svaki oblaka pojedinačno u oblak svojstva konzole za VMM.

13.  Kliknite **Dalje** da biste dovršili postupak. Nakon registracije, metapodataka s poslužitelja VMM se dohvaćaju oporavak Azure web-mjesta. Na kartici **Poslužitelji VMM** na stranici **poslužitelja** u na sigurnog prikazat će se na poslužitelj.

    ![Poslužitelj](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

11. Kada je li poslužitelj dostupan na konzoli za oporavak web-mjesta u **izvoru** > **Priprema izvor** odaberite poslužitelj za VMM i odaberite oblak u kojoj se nalazi na računalo koje hostira Hyper-V. Zatim kliknite **u redu**.

#### <a name="command-line-installation"></a>Instalacija naredbenog retka

Oporavak davatelja za Azure web-mjesta mogu instalirati iz naredbenog retka. Ova metoda se može koristiti da biste instalirali davatelja usluge na poslužitelju Core za Windows Server 2012 R2.

1. Preuzmite tipku davatelja instalacije datoteke i registraciju u mapu. Na primjer C:\ASR.
2. Zaustavljanje servisa Centar sustava Upravitelj virtualnog računala.
3. S dodatnim naredbeni redak, pokrenite te naredbe da biste izdvojili instalacijski program za davatelja usluga:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Pokrenite sljedeću naredbu da biste instalirali davatelja usluga:

        C:\ASR> setupdr.exe /i

5. Izvedite te naredbe da biste registrirali poslužitelja u sigurnog:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Gdje su parametri:

 - **/Credentials**: obavezna parametar koji određuje mjesto u kojoj se nalazi datoteka s ključem Registracija  
 - **/FriendlyName**: obavezna parametar za naziv poslužitelja Hyper-V glavnog računala koja se pojavljuje na portalu za oporavak Azure web-mjesta.
 - **/EncryptionEnabled**: neobavezan parametar koji koristite samo kada replikaciju iz VMM za Azure.
 - **/proxyAddress**: neobavezan parametar koji određuje adresu proxy poslužitelja.
 - **/proxyport**: neobavezan parametar koji određuje priključak proxy poslužitelj.
 - **/proxyUsername**: neobavezan parametar koji određuje Proxy korisničko ime (Ako je proxy poslužitelj zahtijeva provjeru autentičnosti).
 - **/proxyPassword**: neobavezan parametar koji određuje lozinku za provjeru autentičnosti s proxy poslužitelja (Ako je proxy poslužitelj zahtijeva provjeru autentičnosti).  

## <a name="step-3-set-up-the-target-environment"></a>Korak 3: Postavljanje okruženja cilj

Odaberite ciljni VMM server i oblaka.

1. Kliknite **Priprema infrastrukture** > **Ciljna** i odaberite odredišni VMM poslužitelj koji želite koristiti.
2.  Prikazat će se oblaka na poslužitelju koji se sinkroniziraju s oporavak web-mjesta. Odaberite ciljni oblaka.

    ![Cilj](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="step-4-set-up-replication-settings"></a>Korak 4: Postavljanje postavki replikacije

1. Da biste stvorili novi replikacije pravila kliknite **Priprema infrastrukture** > **Replikacije postavke** > **+ Stvori i povezati**.

    ![Mreža](./media/site-recovery-vmm-to-vmm/gs-replication.png)

2. **Stvaranje** i pridruži pravila unesite naziv pravila. Vrsta izvorišno i odredišno mora biti **Hyper-V**.
3. U **verziji za glavno računalo Hyper-V** odaberite koji operacijski sustav je pokrenut na glavnom računalu.

    > [AZURE.NOTE] Oblak VMM može sadržavati Hyper-V domaćini s različitim verzijama (podržani) sustava Windows Server, ali pravilo replikacije je primijenjena domaćini isti operacijski sustav. Ako imate glavnih računala radi više od verzije operacijskih sustava stvoriti pravila za zasebnom replikacije.

4. U web-mjesto **Vrsta provjere autentičnosti** i u okvir za **priključak za provjeru autentičnosti** odredite kako promet provjere autentičnosti između poslužitelja glavno računalo Hyper-V primarni i oporavak. Odaberite **certifikat** osim ako imate radnog okruženja Kerberos. Oporavak Azure web-mjesta automatski će konfigurirati certifikata za provjeru autentičnosti HTTPS. Ne morate ništa učiniti ručno. Prema zadanim postavkama, priključak 8083 i 8084 (za potvrde) će se otvoriti u vatrozidu za Windows na poslužiteljima Hyper-V glavnog računala. Ako ste odabrali **Kerberos**, Kerberos karata će se koristiti za međusobna provjeru autentičnosti poslužitelja glavnog računala. Imajte na umu da ova postavka vrijedi samo za Hyper-V glavnog računala poslužitelja sustavom Windows Server 2012 R2.
3. U **kopirajte učestalost** odredite koliko često replicirati delta podataka nakon početnog replikacije (svakih 30 sekundi, 5 ili 15 minuta).
4. U **oporavak pokažite zadržavanja**, navedite u satima koliko prozoru zadržavanja će biti za svaku točku za oporavak. Zaštićeni strojeva se može oporaviti postavite unutar prozora.
6. U **aplikaciju dosljedan snimke učestalost** unesite koliko se često (1 do 12 sati) oporavak točke koja sadrži aplikaciju dosljedan snimke će biti stvoren. Hyper-V koristi dvije vrste snimaka – standardni snimku zaslona koja nudi rastuće snimku cijelu virtualnog računala i aplikaciju dosljedan snimku zaslona koja stvara točke u vrijeme snimku podataka aplikacije unutar virtualnog računala. Aplikacija dosljedan snimke pomoću servisa Kopiraj glasnoću sjena (VSS) aplikacije provjerite jesu li u dosljedan stanju kada se koristi se snimka. Imajte na umu da Ako omogućite aplikacije dosljedan snimke, jer će utjecati na performanse aplikacije koje rade na virtualnim strojevima izvora. Provjerite je li vrijednost postavljate manji broj točaka dodatne oporavak konfigurirate.
7. U **prijenosa sažimanje podataka** odredite hoće li se sažeti repliciranu podataka koji se prenose.
8. Odaberite **Izbriši replike VM** da biste odredili da se virtualnog računala replike treba izbrisati Ako onemogućite zaštite za izvor VM. Ako omogućite tu postavku, kada onemogućite zaštite za izvor VM znači da je uklonjena iz konzole za oporavak web-mjesta, postavke oporavak web-mjesta u VMM uklonjeni su iz konzole za VMM, a replike se briše.
3. U **način za replikaciju početni** ako ste replikaciju putem mreže navedite želite li da biste započeli početne replikacije ili zakažite. Da biste spremili propusnost mreže možda ćete morati zakazivanje izvan vaše zauzet sati. Zatim kliknite **u redu**.

    ![Pravila replikacije](./media/site-recovery-vmm-to-vmm/gs-replication2.png)

6. Prilikom stvaranja novog pravila ga automatski povezao s VMM oblaka. **Replikacija** pravila kliknite **u redu**. Možete pridružiti dodatne VMM oblaka (i VMs u njima) ovo pravilo za replikaciju u **postavkama** > **replikacije** > naziv pravila > **Pridružiti VMM oblaka**.

    ![Pravila replikacije](./media/site-recovery-vmm-to-vmm/policy-associate.png)

### <a name="prepare-for-offline-initial-replication"></a>Priprema za replikaciju izvan mreže početne

To možete učiniti izvanmrežnu reprodukciju kopije početne podatke. Možete to pripremiti na sljedeći način:

- Na s izvorišnim poslužiteljem ćete Navedite put mjesto s kojeg izvoz podataka se odvija. Dodijelite Potpuna kontrola za NTFS i zajedničko korištenje dozvola za servis VMM na putu izvoz. Na poslužitelju ciljni ćete Navedite put mjesto s kojeg će se pojaviti uvoz podataka. Dodijelite iste dozvole na ovaj put za uvoz.
- Ako se uvoz ili izvoz put se zajednički koristi, dodijelite administratora, Power korisnika, ispis Operator ili Operator poslužitelja članstvo u grupi za račun servisa VMM na udaljenom računalu na kojem zajedničko korištenje nalazi.
- Ako koristite svih računa za pokretanje kao da biste dodali domaćini na putova uvoz i izvoz dodjeljivanje čitanje i dozvole za pisanje račune Pokreni kao u VMM.
- Zajedničko korištenje za uvoz i izvoz treba se nalazi na bilo kojem računalu koji se koristi kao glavno računalo poslužiteljem Hyper-V jer povratna konfiguracije ne podržava Hyper-V.
- U servisu Active Directory na svakom Hyper-V glavnog poslužitelja koja sadrži virtualnim strojevima želite zaštititi, Omogućivanje i konfiguriranje ograničeno delegiranje pouzdanosti udaljenim računalima na kojima putove za uvoz i izvoz nalaze, na sljedeći način:
    1. Na kontrolerom domene otvorite **Active Directory korisnicima i računalima**.
    2. U stablu konzole kliknite **naziv domene** > **računala**.
    3. Desnom tipkom miša kliknite naziv Hyper-V glavnog računala poslužitelja > **Svojstva**.
    4. Na kartici **delegiranje** kliknite **pouzdanost ovo računalo za delegiranje navedeni servisima**.
    5. Kliknite **bilo koji protokol za provjeru autentičnosti**.
    6. Kliknite **Dodaj** > **korisnicima i računalima**.
    7. Upišite naziv računala na kojem je smještena put Izvoz > **u redu**. Na popisu dostupnih usluga, pritisnite i držite tipku CTRL i kliknite **cifs** > **u redu**. Ponovite za naziv računala na kojem je smještena put uvoz. Potrebi ponovite ove korake za dodatne poslužitelje Hyper-V glavnog računala.


### <a name="configure-network-mapping"></a>Konfiguriranje mapiranja mreže

Postavljanje preslikavanje između izvorišno i odredišno mreže.

- [Čitanje](#prepare-for-network-mapping) za brzi pregled koje preslikavanje ne. U zbrajanja [čitati](site-recovery-network-mapping.md) za detaljnije objašnjenje.
- Provjerite je li virtualnim strojevima na poslužiteljima VMM povezani s mrežom VM.

Konfiguriranje mapiranja na sljedeći način:

1. **Postavke** > **Infrastruktura za oporavak web-mjesta** > **Mreže mapiranja** > **mapiranja mreže** kliknite **+ mapiranja mreže**.

    ![Preslikavanje](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Na kartici **Dodaj preslikavanje** odaberite izvor i ciljnog VMM poslužiteljima. Mreža VM povezan s poslužiteljima VMM učitavaju.
3. U odjeljku **mreža izvor**odaberite mreže koju želite koristiti s popisa VM mreže povezane s primarnom VMM poslužitelju.
6. U **mreži cilj** odaberite mreže koju želite koristiti sekundarne VMM poslužitelja. Zatim kliknite **u redu**.

    ![Preslikavanje](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Evo što se događa kada započne preslikavanje:

- Bilo koje postojeće replike virtualnim strojevima koji odgovaraju na mrežu VM izvora će biti povezani s mrežom VM cilj.
- Novi virtualnim strojevima koji su povezani s mrežom VM izvora će se povezati s mreže cilj mapirani nakon replikacije.
- Ako mijenjate postojeći mapiranje pomoću nove mreže, replike virtualnim strojevima će se povezati pomoću nove postavke.
- Ako mreža cilj ima više podmreže i jednu od tih podmreže ima isti naziv kao podmreže na kojem se nalazi virtualnog računala izvora, zatim virtualnog računala replike će se povezati s tom podmreže ciljne nakon prebacivanje. Ako nema cilj podmreže pod nazivom koji se podudaraju, virtualnog računala će se povezati s prvom podmreže u mreži.

### <a name="configure-storage-mapping"></a>Konfiguriranje mapiranja prostora za pohranu
Prema zadanim postavkama kada replicirati virtualnog računala na poslužitelj Hyper-V izvora na poslužitelju ciljni Hyper-V glavno računalo, repliciranu podaci se pohranjuju na zadano mjesto naznačenu za ciljno glavno računalo Hyper-V u upravitelju Hyper-V. Za veću kontrolu nad pohranjuju repliciranu podataka, možete konfigurirati za pohranu mapiranja<br/><br/> Da biste konfigurirali mapiranja prostora za pohranu, morate postavili klasifikacije prostora za pohranu na izvoru i ciljnog VMM poslužitelje prije nego što počnete implementacije. Trenutno mapiranje prostora za pohranu putem portala za nove Azure nije podržano. Međutim, možete je omogućiti PowerShell. [Dodatne informacije](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-6-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>Korak 5: Planiranje kapaciteta

Sad kad ste na basic infrastrukture postavili ste možete razmislite o planiranje kapaciteta i utvrđivanje trebate li dodatne resurse.

Oporavak web-mjesta omogućuje programa Excel temelji kapaciteta planer da vam dodijeliti prave resurse za vaše okruženje izvora, komponente oporavak web-mjesta, mreže i prostora za pohranu. Planer možete pokrenuti u brzom načinu rada za Procjena Prosječan broj VMs, diskova i prostora za pohranu na temelju ili u detaljnom načinu kojim ćete slika na razini radno opterećenje za unos. Prije nego što počnete morat ćete:

- Prikupite informacije o okruženju sustava replikacije, uključujući VMs diskova po VMs i prostor za pohranu po disk.
- Procjenu dnevnih stopa promjena (churn) morat ćete repliciranu delta podataka. [Planer kapaciteta za Hyper-V replike](https://www.microsoft.com/download/details.aspx?id=39057) možete koristiti da biste mogli učiniti ovo.

1.  Kliknite **Preuzimanje** Preuzmite alat, a zatim ga pokrenuti. [U članku čitanje](site-recovery-capacity-planner.md) koja se isporučuje se uz alat.
2.  Kada završite odaberite **da** **ste pokrenuli planer kapaciteta**?

    ![Planiranje kapaciteta](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="control-bandwidth"></a>Kontrola propusnosti

Nakon što ste prikupili podatke o replikacije delta u stvarnom vremenu pomoću tehnologije Hyper-V replike planer kapaciteta, Planer alat koji se temelji na Excel – kapaciteta pomaže vam za izračun propusnosti vam je potrebna za replikaciju (inicijala i delta). Omogućuje kontrolu količine propusnost za replikaciju možete konfigurirati pravila NetQos pomoću pravila grupe ili komponente Windows PowerShell. To možete učiniti na nekoliko načina:

**PowerShell** | **Pojedinosti**
--- | ---
**Novi - NetQosPolicy "QoS u odredišnoj podmreži"** | Throttle promet s Hyper-V glavnog računala sa sustavom Windows Server 2012 R2 sekundarne podmreže. Koristite ako se vaš primarnih i sekundarnih podmreže razlikuju.
**Novi - NetQosPolicy "QoS odredišni priključak"** | Throttle promet s odredišnim priključkom sa sustavom Windows Server 2012 R2 Hyper-V glavnog računala.
**Novi - NetQosPolicy "Ograničenja promet s VMM"** | Throttle promet s vmms.exe. To će throttle Hyper-V promet i Live promet za migraciju. Možete povezati IP protokoli i priključke za preciznije određivanje.

Možete koristiti propusnosti Debljina postavke ili ograničiti promet bita po sekundarni. Ako koristite klaster morate učiniti na sve čvor klaster. Dodatne informacije potražite u člancima:


- Pošta Maurer blog na [Ograničavanje promet replike Hyper-V](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/)
- Dodatne informacije o [cmdleta New-NetQosPolicy](https://technet.microsoft.com/library/hh967468.aspx).


## <a name="step-6-enable-replication"></a>Korak 6: Omogući replikacije

Sada omogućiti replikaciju na sljedeći način:

1. Kliknite **Korak 2: replicirati aplikacije** > **izvora**. Kada omogućite replikacije prvi put, kliknite **+ replicirati** u sigurnog da biste omogućili replikacije za dodatnih računala.

    ![Omogućivanje replikacije](./media/site-recovery-vmm-to-vmm/enable-replication1.png)

2. U plohu **izvora** > odaberite poslužitelj VMM poslužitelja u oblak i u kojem se nalaze domaćini Hyper-V želite replicirati. Zatim kliknite **u redu**.

    ![Omogućivanje replikacije](./media/site-recovery-vmm-to-vmm/enable-replication2.png)

3. U **ciljnom** plohu provjerite sekundarni poslužitelj VMM i oblaka.
4. Na **virtualnim računalima sustava** odaberite VMs koji želite zaštititi s popisa.

    ![Omogući zaštitu virtualnog računala](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

Možete pratiti napredak akciju **Omogućite zaštite** u postavke > **poslove** > **poslove oporavak web-mjesta**. Nakon izvođenja posla **Dovršavanje zaštitu** virtualnog računala spremna je za prebacivanje.


>[AZURE.NOTE] Možete omogućiti i zaštitu virtualnim strojevima na konzoli VMM. Kliknite **Omogući zaštitu** na alatnoj traci u svojstva virtualnog računala > kartica **Oporavak Azure web-mjesta** .

Kada omogućite replikacije možete prikazati svojstva za VM u **postavkama** > **Replicirati stavke** > vm naziv. Na nadzornoj ploči za **Essentials** možete vidjeti informacije o pravilima za replikaciju u VM i njezin status. Dodatne informacije, kliknite **Svojstva** .

### <a name="onboard-existing-virtual-machines"></a>Onboard postojeće virtualnim strojevima

Ako imate postojeću virtualnim strojevima u VMM koji su replikaciju s Hyper-V replike možete se ih za zaštitu oporavak Azure web-mjesta na sljedeći način:

1. Provjerite je li poslužitelj Hyper-V koji hostira postojeće VM nalazi u primarni oblaka i Hyper-V poslužitelj koji hostira virtualnog računala replike nalazi u oblaku sekundarne.
2. Provjerite je li pravilo replikacije konfiguriran za primarni VMM spremanja.
2. Omogućivanje replikacije za primarni virtualnog računala. Azure oporavak web-mjesta i VMM će provjerite otkrije isti replike glavno računalo i virtualnog računala i oporavak web-mjesta Azure će ponovno iskoristiti i ponovno uspostaviti replikacije pomoću navedeni postavki.


## <a name="step-7-test-your-deployment"></a>Korak 7: Testiranje implementaciju sustava

Da biste testirali implementaciju sustava pokrenite test prebacivanje za jednu virtualnog računala ili stvorite plan oporavak koja sadrži jedan ili više virtualnim računalima.

### <a name="prepare-for-failover"></a>Priprema za prebacivanje

- Da biste potpuno testiranje implementaciju sustava morate do infrastrukture za repliciranu strojno funkcionirati prema očekivanjima. Ako želite testirati servisa Active Directory i DNS možete stvoriti virtualnog računala kao kontroler domene s DNS-a i replicirati to Azure pomoću oporavak Azure web-mjesta. Pročitajte više u [test prebacivanje zahtjevi za Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Upute u ovom se članku opisuje način da biste pokrenuli test prebacivanje s nema mreže. Ta mogućnost će testirajte na VM ne uspije iznad, ali nećete testirajte postavke mreže s VM. [Saznajte više](site-recovery-failover.md#run-a-test-failover) o ostalim mogućnostima.
- Ako želite pokrenuti neplanirano prebacivanje umjesto test prebacivanje Imajte na umu sljedeće:

    - Ako je to moguće isključite primarni strojeva prije pokretanja neplanirano prebacivanje. Na taj način nemate izvora i replike računala izvode u isto vrijeme.
    - Kada pokrenete neplanirano prebacivanje prestane replikacije podataka iz primarnog strojeva tako da sve delta podataka neće prenijeti nakon neplanirano prebacivanje započinje. Nadalje ako pokrenete neplanirano prebacivanje na tarifu za oporavak funkcionirat će dok se ne dovrši, čak i ako dođe do pogreške.




### <a name="run-a-test-failover"></a>Pokrenite test prebacivanje

1. Neuspjeh putem jednog VM u **postavkama** > **Replicirati stavki**, kliknite na VM > **+ Prebacivanje Test**.

    ![Prebacivanje test](./media/site-recovery-vmm-to-vmm/test-failover.png)

2. Neuspjeh putem tarifu za oporavak, u odjeljku **Postavke** > **Oporavak tarife**, desnom tipkom miša kliknite željenu tarifu > **Prebacivanje Test**. Da biste stvorili na oporavak planiranje [, slijedite ove upute](site-recovery-create-recovery-plans.md).
2. **Prebacivanje Test**, odaberite **nema**. Uz tu se mogućnost testirajte na VM ne uspije putem prema očekivanjima, ali repliciranu VM neće biti povezani s mreže.

    ![Prebacivanje test](./media/site-recovery-vmm-to-vmm/test-failover1.png)

3. Kliknite **u redu** da biste započeli s pomoćnim. Možete pratiti napredak tako da kliknete na VM da biste otvorili svojstva ili na poslu **Test prebacivanje** u **postavkama** > **poslove** > **poslove oporavak web-mjesta**.
4. Kada posao prebacivanje dosegne faza **testiranje dovrši** , učinite sljedeće:

    -  Prikaz replike VM sekundarne VMM oblaka.
    -  Kliknite **dovrši test** da biste dovršili postupak prebacivanje test.
    -  Kliknite **bilješke** da biste snimili i spremili sve opažanja povezan s pomoćnim test.

5. Na glavnom računalu isti kao glavno računalo replike virtualnog računala postoji stvorit će se test virtualnog računala. Ono se dodaje u istom oblak u kojoj se nalazi replike virtualnog računala.
6. Nakon potvrđivanja koji se pokreću VMs uspješno kliknite **Prebacivanje test je dovršena**. U ovoj fazi brišu se svi elementi automatski stvorio oporavak web-mjesta tijekom prebacivanje test.  

    > [AZURE.NOTE] Ako testiranje prebacivanje više od dva tjedna nastavlja se dovrši po prisilno.



### <a name="update-dns-with-the-replica-vm-ip-address"></a>Ažurirajte DNS replike VM IP adresa

Nakon prebacivanje replike VM možda neće imati istu IP adresu kao primarni virtualnog računala.

- Virtualnim strojevima će se ažurirati DNS poslužitelju na kojem se koriste kada počnu.
- Možete i ručno ažurirati na DNS-a na sljedeći način:

#### <a name="retrieve-the-ip-address"></a>Dohvaćanje IP adrese

Pokrenite ovaj primjer skripte za dohvaćanje IP adrese.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="update-dns"></a>Ažuriranje DNS-a

Pokrenite ovaj primjer skripte za ažuriranje DNS-a, pri određivanju IP adresu koju ste vratili pomoću prethodni primjer skripte.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

## <a name="next-steps"></a>Daljnji koraci

Nakon uvođenja postavljen i radi, [Dodatne informacije](site-recovery-failover.md) o različitim vrstama failovers.
