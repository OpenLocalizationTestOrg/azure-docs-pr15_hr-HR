<properties
    pageTitle="Mrežni nadzor performansi rješenja u OMS | Microsoft Azure"
    description="Nadzornik performansi mreže olakšava praćenje učinkovitosti vaše mreža u blizini real-time za otkrivanje i pronađite mreže grla performansi."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="banders"/>

# <a name="network-performance-monitor-preview-solution-in-oms"></a>Mrežni rješenje OMS performansi monitora (pretpregled)

>[AZURE.NOTE] To je [rješenje za pretpregled](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Ovaj dokument opisuje kako postaviti i korištenje nadzornik performansi mreže rješenja u OMS koji olakšava praćenje učinkovitosti vaše mreža u blizini real-time za otkrivanje i pronađite mrežne grla performansi. Uz rješenje nadzornik performansi mreže možete nadzirati gubitka i latencije između dvije mreže, podmreže ili poslužitelji. Nadzornik performansi mreže problema s mrežom kao što su blackholing promet, usmjeravanje pogrešaka i problemima koji su nadzora metode običan mreže mogu otkriti. Nadzornik performansi mreže stvara upozorenja i obavještava kao i kada je praga breached za mrežnu vezu. Ove pragovi može biti naučili automatski sustav ili možete konfigurirati tako da koristi prilagođeni upozorenja pravila. Nadzornik performansi mreže osigurava pravovremeno otkrivanje probleme s performansama mreže i localizes izvor problema segmenta određeni mreže ili uređaj.

Problemi s nadzorne ploče rješenja koja prikazuje sažete informacije o vašoj mreži uključujući nedavne događaje stanje mreže, dobro mreže veze i subnetwork veze koje su nasuprotne visoke gubitak i Latencija mrežom može otkriti. Koje se naniže u mrežnu vezu da biste prikazali trenutni status stanja veze subnetwork, kao i veze čvor čvor. Možete pogledati i povijesnim trend gubitka i Latencija na mreži, subnetwork i na razini čvor čvora. Možete otkriti problema s mrežom tranzitne pregledom povijesne trend grafikone za gubitak paketa i Latencija i pronađite grla mreže na karti topologije. Interaktivni topologije grafikonu omogućuje vizualni prikaz usmjerava skoka po skoka mreže i određivanje izvor problema. Kao što je sve druge mogućnosti zapisnika pretraživanja za različite preduvjeti analize možete koristiti da biste stvorili prilagođena izvješća na temelju podatke prikupljene putem nadzornik performansi mreže.

Rješenje koristi stilova sintetičkih transakcije kao primarni mehanizam za otkrivanje kvarove mreže. Tako, možete je koristite bez regard dobavljača ili modela određene mrežni uređaj. To funkcionira na lokalni, oblaka (IaaS) i hibridnog okruženja. Rješenje se automatski otkriva mrežna Topologija i razne usmjerava u vašoj mreži.

Nadzor proizvodi mreža usredotočite se na uređaj (usmjerivača, parametri itd.) stanje mreže za nadzor, no nudi uvida u stvarni kvalitete mrežne veze između dvije točke koji ne nadzornik performansi mreže.

### <a name="using-the-solution-standalone"></a>Korištenje samostalne rješenja

Ako želite nadzirati kvalitete mrežnih veza između njihove ključnih radnih opterećenja mrežama, podatkovnim centrima ili web-mjesta sustava office, a zatim koje možete koristiti rješenja nadzornik performansi mreže sam praćenje stanja povezivanje između:

- više podatkovnim centrima ili office web-mjesta koji su povezani pomoću javna ili privatna mreža
- ključna radnih opterećenja koji su pokrenuti poslovnim aplikacijama
- Javni oblaka servisima kao što su Microsoft Azure ili Amazon Web Services (AWS) i lokalne mreže, ako imate IaaS (VM) dostupan, a imate pristupnika konfigurirano tako da omogućuje komunikaciju između lokalne mreže i mreža oblaka
- Azure i lokalnih mreža kada koristite usmjeravanje Express

### <a name="using-the-solution-with-other-networking-tools"></a>Rješenje pomoću druge alate za umrežavanje

Ako želite nadzirati redak poslovnoj aplikaciji, možete koristiti rješenja nadzornik performansi mreže kao rješenje pomoćnom druge alate za mrežu. Kada je mreža spora može dovesti do sporo aplikacije i nadzornik performansi mreže omogućuju Istraživanje problema s performansama aplikacije koje uzrokuju u podlozi mrežnih problema. Jer je rješenje zahtijevaju pristup mrežne uređaje, administratora aplikacije nije potrebno je za umrežavanje tim saznat ćete kako mreže je utjecaja aplikacije.

Osim toga, ako već ulažete u druge alate za nadzor mreže, zatim rješenje možete nadopunjuju tih alata jer Većina tradicionalni rješenja za nadzor mreže daje uvid u metriku performanse mreže završetka do kraja kao što su gubitka i Latencija.  Rješenje nadzornik performansi mreže može pridonijeti ispune te razmak.


## <a name="installing-and-configuring-agents-for-the-solution"></a>Instaliranje i konfiguriranje agenata rješenja

Instalirajte agenata na [računalima povezivanje Windows zapisnika analize](log-analytics-windows-agents.md) i [Povezivanje prijava analitiku u komponente Operations Manager](log-analytics-om-agents.md)pomoću procesa basic.

>[AZURE.NOTE]
Morat ćete instalirati barem 2 agenata da bi se dovoljno podataka da biste otkrili i nadzor vaši mrežni resursi. U suprotnom rješenje će ostati u konfiguriranje stanje dok ne instalirate i konfigurirate dodatne agenata.

### <a name="where-to-install-the-agents"></a>Mjesto za instalaciju na agenata

Prije instalacije agenata, razmislite o topologiji mreže te dijelovi mreže koju želite nadzirati. Preporučujemo da instalirate više agente za svaki podmreže koju želite nadzirati. Drugim riječima, za svaku podmreže koju želite nadzirati, odaberite dvije ili više poslužitelja ili VMs i instalirajte agenta na njima.

Ako niste sigurni topologija mreže, instalirajte na agente na poslužiteljima s ključnih radnih opterećenja mjesto na koje želite nadzirati mrežnih performansi. Na primjer, možda ćete da biste pratili mrežnu vezu između web-poslužitelj i poslužitelja na kojem je SQL Server. U ovom primjeru želite instalirati agent na poslužiteljima.

Agente praćenje veza s mrežom (veze) između domaćini – ne domaćini sami. Tako da biste pratili mrežnu vezu, morate instalirati agenata na oba krajnje točke te veze.

### <a name="configure-agents"></a>Konfiguriranje agenata

Nakon instalacije agenata, morat ćete otvoriti priključaka u vatrozidu za tih računala da bi mogli komunicirati agenata. Morate preuzeti i pokrenuti [skriptu EnableRules.ps1 PowerShell](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) bez parametre u prozoru PowerShell s administratorskim ovlastima

Skripta stvara ključevima registra potrebnih nadzornik performansi mreže i stvara pravila vatrozida za Windows da biste omogućili agenata da biste stvorili veze TCP međusobno. Ključevi registra stvorio skriptu navedite želite li da biste se prijavili zapisnika pogrešaka i put do datoteke zapisnika. Određuje i priključak TCP agent koji se koristi za komunikaciju. Vrijednosti za ove tipke automatski postaviti skripta, pa ne morate ručno promijeniti ove tipke.

Priključak otvoriti po zadanom je 8084. Možete koristiti prilagođene priključak unosom parametar `portNumber` u skriptu. Međutim, isti priključak moraju se koristiti na svim računalima gdje je pokrenuti skriptu.

>[AZURE.NOTE] Skripta EnableRules.ps1 konfigurira pravila vatrozida za Windows samo na računalu na kojem se izvodi skriptu. Ako imate mrežni vatrozid, provjerite je li to dopušta promet namijenjene prikazivanju na TCP priključak koristi nadzornik performansi mreže.


## <a name="configuring-the-solution"></a>Konfiguriranje rješenja

Poslužite se sljedećim informacijama za instalaciju i konfiguriranje rješenja.

1. Rješenje nadzornik performansi mreže nabaviti podataka s računala sa sustavom Windows Server 2008 SP 1 ili noviji ili Windows 7 SP1 ili noviji, koji preduvjeti isti kao u programu Microsoft nadzor Agent (MMA).
2. Dodavanje rješenja nadzornik performansi mreže OMS radni prostor pomoću postupak opisan u [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md).  
  ![Simbol monitora performanse mreže](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. Na portalu OMS vidjet ćete novu pločicu pod naslovom **Nadzornik performansi mreže** s porukom *rješenje potrebna dodatna konfiguracija*. Morat ćete konfigurirati rješenja da biste dodali mreže na temelju subnetworks i čvorove koji su otkriti agenata. Kliknite **Nadzornik performansi mreže** da biste pokrenuli konfiguriranje zadane mreže.  
  ![rješenje potrebna dodatna konfiguracija](./media/log-analytics-network-performance-monitor/npm-config.png)


### <a name="configure-the-solution-with-a-default-network"></a>Konfiguriranje rješenja uz zadane mreže

Na stranici Konfiguracija vidjet ćete jedne mreže pod nazivom **zadano**. Ako još niste definirana nijednu mrežu, sve se automatski otkriti podmreže spremaju se u zadane mreže.

Svaki put kada stvorite mrežu, možete dodati podmreži za, a taj podmreže je uklonjena iz zadane mreže. Ako ste izbrisali s mrežom, njegov podmreže automatski vraćaju kao zadane mreže.

Drugim riječima, zadane mreže je spremnik za sve podmreže koje se nalaze u svaka mreža za korisnički definirane. Nije moguće uređivati ni brisati zadane mreže. Uvijek se ostaje u sustavu. No možete stvoriti mreža koliko god želite.

U većini slučajeva, podmreže u tvrtki ili ustanovi će biti raspoređeni u više od jedne mreže i potrebno stvoriti jednu ili više mreža da biste logično grupirali vaše podmreže.

### <a name="create-new-networks"></a>Stvaranje nove mreža

Mreža u nadzornik performansi mreže je spremnik za podmreže. Možete stvoriti u mreži s bilo koji naziv i dodajte podmreže s mrežom. Ako, na primjer, možete stvoriti u mreži s nazivom *Building1* i dodavanje podmreže ili možete stvoriti u mreži s nazivom *DMZ* i dodati sve podmreže pripadaju demilitarized zone s mrežom.

#### <a name="to-create-a-new-network"></a>Da biste stvorili novu mrežu

1. Kliknite **Dodaj mrežu** , a zatim upišite naziv mreže i opis.
2.  Odaberite jednu ili više podmreže, a zatim kliknite **Dodaj**.
3. Kliknite **Spremi** da biste spremili konfiguracije.  
  ![Dodavanje mreže](./media/log-analytics-network-performance-monitor/npm-add-network.png)



### <a name="wait-for-data-aggregation"></a>Pričekajte za zbrajanje podataka

Nakon spremanja konfiguraciju prvi put rješenje pokreće prikupljanje informacija gubitka i Latencija mreže paketa između čvorove instaliranim agenata. Taj postupak može potrajati, ponekad 30 minuta. Tijekom to stanje pločicu nadzornik performansi mreže na stranici pregled prikazuje poruku *prikupljanja podataka u tijeku*.

![Zbrajanje podataka u tijeku](./media/log-analytics-network-performance-monitor/npm-aggregation.png)


Prilikom prijenosa podataka prikazat će vam se prikazuje nadzornik performansi mreže pločica ažurirati podatke.

![Pločica nadzor performansi mreže](./media/log-analytics-network-performance-monitor/npm-tile.png)

Kliknite pločicu da biste pogledali na nadzornoj ploči nadzornik performansi mreže.

![Nadzorna ploča za nadzor performansi mreže](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Uređivanje postavki nadzora za podmreže

Sve podmreže instaliranim barem jedan agent navedene su na kartici **Subnetworks** na stranici Konfiguracija.

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>Omogućivanje i onemogućivanje nadzor za određeni subnetworks

1. Odaberite ili poništite okvir pokraj **subnetwork ID** , a zatim provjerite je li **namijenjen je nadzor** odabrane ili očišćenog, sukladno situaciji. Možete potvrdite ili poništite više podmreže. Kada je onemogućen, subnetworks ne provjeravaju kao na agente ažurirat će se zaustaviti pinging druge agente.
2. Odaberite čvorove koju želite pratiti za određeni subnetwork, na popisu odaberete u subnetwork i premjestite potrebna čvorove između popisa koji sadrži automatiziranog i nadziranim čvorove.
Možete dodati prilagođene **Opis** subnetwork, po želji.
3. Kliknite **Spremi** da biste spremili konfiguracije.  
  ![Uređivanje podmreže](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>Odaberite čvorove praćenje

Na kartici **čvorove** navedene su sve čvorove koji imaju agent instalirana na njima.

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>Omogućivanje i onemogućivanje nadzora za čvorove

1. Potvrdite ili poništite čvorove koje želite nadzirati ili prestane nadzirati.
2. Kliknite **koristi za nadzor**ili ga poništite po potrebi.
3. Kliknite **Spremi**.  
  ![Omogućite praćenje čvor](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)


### <a name="set-monitoring-rules"></a>Postavljanje pravila za nadzor

Nadzornik performansi mreže generira stanja događaja o veze između par čvorove ili subnetwork ili mrežne veze kada je breached praga. Ove pragovi može biti naučili automatski sustav ili ih možete konfigurirati prilagođena upozorenja pravila.

*Zadana pravila* stvara sustava i stvara stanja događaj kad god gubitka ili latencije između par mrežama ili subnetwork povezuje breaches sustava naučili praga. Možete odabrati da biste onemogućili zadane pravilo, a zatim stvaranje prilagođenog pravila nadzora

#### <a name="to-create-custom-monitoring-rules"></a>Da biste stvorili prilagođeni nadzora pravila

1. Kliknite **Dodaj pravilo** na kartici **Monitor** , a zatim unesite naziv pravila i opis.
2. Odaberite par mreže ili subnetwork veze praćenje s popisa.
3. Najprije odaberite mreža u kojoj se nalazi prvi subnetwork/s interesa na padajućem izborniku mreže, a zatim odaberite subnetwork/s na padajućem izborniku odgovarajuće subnetwork.
Ako želite nadzirati subnetworks u mrežnu vezu, odaberite **sve subnetworks** . Isto tako odaberite na druge subnetwork/s koji vas zanimaju. Možete i kliknete **Dodavanje iznimke** da biste izuzeli nadzor za određeni subnetwork veze iz odabira koje ste učinili.
4. Ako ne želite da biste stvorili događaje stanja za stavke koje ste odabrali, zatim poništite **Omogućivanje stanja nadzor na veze prekriveni ovo pravilo**.
5. Odaberite uvjete za nadzor.
Prilagođeni pragovi za generiranje događaja stanja možete postaviti tako da upišete vrijednosti praga. Kad god vrijednost uvjet dolazi iznad praga odabrani za par odabrane mreže/subnetwork, generirat će se stanje događaj.
6. Kliknite **Spremi** da biste spremili konfiguracije.  
  ![Stvaranje prilagođenog pravila nadzora](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)


## <a name="data-collection-details"></a>Detalji zbirke podataka

Nadzornik performansi mreže koristi TCP SYN-SYNACK-ACK rukovanja pakete da biste prikupili gubitka i Latencija informacije i traceroute se koristi za dohvaćanje informacija o topologiji.

Sljedeća tablica prikazuje metode zbirke podataka i druge detalje o načinu prikupljanja podataka za nadzor performansi mreže.

| platforme | Izravni Agent | Agent za SCOM | Azure prostora za pohranu | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
| Windows |![Da](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Da](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![ne](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|            ![ne](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|![ne](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)| TCP handshakes svakih 5 sekundi podataka poslane svake 3 minute |

Rješenje čini pomoću stilova sintetičkih transakcija procijenite stanje mreže. OMS agenata instalirati na razne točke u mreži exchange TCP paketi međusobno, a zatim u postupak, Saznajte trajanjem putovanja vrijeme i paketa gubitak, ako postoje. Povremeno svaki pojedini agent izvodi i praćenje rute do druge agenata možete pronaći sve različite usmjerava na mrežu s kojom se moraju testirati. Pomoću ove podatke u agenata su razlučiti latenciju mreže i slika gubitak paketa. Testova se ponavljaju svakih pet sekundi, a podaci se pridružuje neko od tri minute tako da na agente prije prijenosa na OMS.

>[AZURE.NOTE] Iako agente međusobno komunicirati često, oni se neće prikazivati mnogo mrežni promet tijekom provođenju testova. Agenata je samo za pakete rukovanja TCP SYN-SYNACK-ACK da biste odredili gubitka i Latencija – podaci su razmijenili pakete. Tijekom tog postupka agenata međusobno komunicirati samo kad je potreban i komunikacije topologije agent optimiziran je da biste smanjili mrežni promet.

## <a name="using-the-solution"></a>Korištenje rješenja

U ovom se odjeljku objašnjava sve na nadzornoj ploči funkcije i kako ih koristiti.

### <a name="solution-overview-tile"></a>Pločica pregled rješenja

Kada omogućite rješenje nadzornik performansi mreže, pločicu rješenje na stranici pregled OMS sadrži kratak pregled stanja mreže. Prikazuje prstenasti grafikon prikazuje broj dokumenata i dobro subnetwork veza. Kada kliknete pločicu, otvara se na nadzornoj ploči rješenja.

![Pločica nadzor performansi mreže](./media/log-analytics-network-performance-monitor/npm-tile.png)


### <a name="network-performance-monitor-solution-dashboard"></a>Nadzorna ploča za rješenja nadzor performansi mreže

**Mrežni sažetak** plohu prikazuje sažetak mreža uz veličinu relativni. To slijedi pločice koje se prikazuje ukupni broj veze s mrežom, podmreže veze i putove u sustavu (puta sastoji se od IP adrese dva domaćini s agenata i preskakanja između njih).

Plohu **Vrha mreže stanja događaje** daje popis najnovije stanje događaja i upozorenja u sustavu te vrijeme nakon događaja active. Stanje događaj ili upozorenje o generira se kad god gubitak ili Latencija veze mreže ili subnetwork premašuje praga.

Plohu **Vrha dobro mreže veze** prikazat će se popis veza dobro mreže. Ovo su veze mreže koje imaju jedan ili više događaja š stanja za njih u trenutku.

**Veze na vrhu Subnetwork najčešće gubitka** i **Subnetwork s najviše Latencija** blades prikazuju gornjoj subnetwork veze prema gubitak i veze na vrhu subnetwork način Latencija odnosno. Visokom latencijom ili neke količinu gubitak možda očekuje na neke veze s mrežom. Veza se prikazuju na vrhu deset popisima, ali nisu označene dobro.

**Uobičajene upite** plohu sadrži skup upita za pretraživanje dohvaćati neobrađenog izravno podataka za nadzor mreže. Te upite možete koristiti kao početnu točku za stvaranje vlastitog upita za prilagođene izvješćivanje.

![Nadzorna ploča za nadzor performansi mreže](./media/log-analytics-network-performance-monitor/npm-dash01.png)


### <a name="drill-down-for-depth"></a>Dubinske analize prema dolje za dubine

Možete kliknuti različite veze na rješenje nadzornu ploču da biste naniže dublju u bilo kojem područje interesa. Na primjer, kada se prikaže upozorenje ili je dobro mrežnu vezu prikazuju se na nadzornoj ploči, možete kliknuti da biste istražili Dodatno. Ćete bili preusmjereni na stranicu koja sadrži popis svih veza subnetwork veze određeni mreže za. Će moći vidjeti status gubitka, Latencija i stanje svake veze subnetwork i brzo pretraživanje koje subnetwork vezama uzrokuju problem. Sada možete kliknuti i **Prikaz čvor veza** da biste vidjeli sve veze čvor dobro podmreže vezu. Nakon toga možete vidjeti pojedinačne čvor čvor veze i pronaći veze na dobro čvor.

Možete kliknuti **topologije prikaz** da biste pogledali topologije skoka po skoka usmjerava između čvorove izvorišne i odredišne. Dobro usmjerava ili preskakanja prikazuju se u crvenoj boji tako da brzo možete prepoznati problem na određeni dio mreže.

![dubinske analize podataka](./media/log-analytics-network-performance-monitor/npm-drill.png)


#### <a name="trend-charts"></a>Trend grafikoni

Na svakoj razini koju ste kroz razine naniže, vidjet ćete trend gubitka i Latencija za mrežnu vezu. Trend grafikone i dostupni su za Subnetwork i čvor veze. Vremenski interval za grafikonu radi iscrtati pomoću kontrola za vrijeme pri vrhu grafikona možete promijeniti.

Trend grafikoni prikazuju povijesnim perspektive performansi mrežnu vezu. Nekih problema s mrežom su tranzitne u prirode, a biti teško Uhvatite samo tako da pogledate trenutno stanje mreže. To je zato problema možete brzo ponuditi i nestaju prije no što svima obavijesti koje, samo da biste se ponovno pojaviti na noviji točki u vremenu. Takve tranzitne probleme može biti teško administratori aplikacije jer se one problemi često površina kao neobjašnjivo povećanja u aplikaciju reakcija, čak i kada svih komponenti aplikacije izgledaju funkcionirati.

Jednostavno može prepoznati te vrste problema tako da pogledate trend grafikon na kojem se problem će kao iznenadno šiljka latenciju mreže ili gubitak.

![trend grafikona](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>Topologija skoka po skoka karte

Nadzornik performansi mreže prikazuje mjesto tako da skoka topologije usmjerava između dva čvorove na programa karte interaktivni topologije. Topologija karte možete pogledati tako da odaberete čvor vezu, a zatim kliknete **Prikaz topologije**. Osim toga, možete pogledati karti topologije tako da kliknete pločicu **putova** na nadzornoj ploči. Kada kliknete **putova** na nadzornoj ploči, morat ćete odaberite čvorove izvorišne i odredišne s ploče na lijevoj strani, a zatim kliknite **Crtanje** da biste iscrtali usmjerava između dva čvorove.

Karta topologije prikazuje koliko usmjerava su između dva čvorove i što putova poduzeti podatkovne pakete. Grla performanse mreže su označeni crvenom crtom na karti topologije. Neispravni mrežne veze ili neispravan mrežnog uređaja možete pronaći tako da pogledate crvena boja elemenata na karti topologije.

Kada na karti topologije kliknete čvor ili postavite pokazivač miša preko njega, vidjet ćete svojstva čvor kao što su FQDN i IP adresom. Kliknite mjesto da biste saznali je IP adresa. Možete istaknuti određeni usmjerava poništavanjem, a zatim odaberite usmjerava koji želite istaknuti na karti. Pomoću sustava kotačića možete Povećavanje i smanjivanje karti topologije.

Imajte na umu da topologije prikazane na karti topologije layer 3 i ne smije sadržavati sloja 2 uređaje i veze.

![Topologija skoka po skoka karte](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Lokalizacija kvara

Nadzornik performansi mreže se može pronaći grla mreže bez povezivanja za mrežne uređaje. Na temelju podataka koji prikuplja s mreže i primjenom napredne algoritama na grafikonu mreže nadzornik performansi mreže čini probabilistic procjenu dijelova mreže koji se najčešće izvor problema.

Taj se način je korisno za određivanje grla mreže kada pristup preskakanja nije dostupna jer ona nije potreban sve podatke da biste se prikupili iz mrežne uređaje kao što su usmjerivača ili parametri. To je korisno kada preskakanja između dva čvorove nisu administratorske kontrole. Na primjer, u preskakanja možda usmjerivača davatelja internetskih usluga.

### <a name="log-analytics-search"></a>Prijava analitiku pretraživanja

Sve podatke koji su grafički prikazanog putem nadzorne ploče nadzornik performansi mreže i naniže stranice dostupan je i nativno u analize zapisnika pretraživanja. Možete podataka pomoću jezika za upite pretraživanja za upite i stvoriti prilagođena izvješća pomoću izvoza podataka u Excel ili PowerBI. **Uobičajene upite** plohu na nadzornoj ploči sadrži neke korisne upite koje možete koristiti kao početnu točku za stvaranje vlastitog upita i izvješća.

![upite pretraživanja](./media/log-analytics-network-performance-monitor/npm-queries.png)


## <a name="investigate-the-root-cause-of-a-health-alert"></a>Istražiti uzrok s upozorenjem o stanju

Sad kad ste pročitali o nadzornik performansi mreže, pogledajmo jednostavne istrage u korijenskog uzroka za događaj stanja.

1. Na stranici pregled dobit ćete brze snimke stanja mrežu tako da opažanja **Nadzornik performansi mreže** pločica. Obratite pozornost na to iz 80 subnetworks veze prate, 43 jesu li dobro. To opravdava istrage. Kliknite pločicu da biste pogledali na nadzornoj ploči rješenja.  
  ![Pločica nadzor performansi mreže](./media/log-analytics-network-performance-monitor/npm-investigation01.png)

2. Primjer slike u nastavku, primijetit ćete da postoje trenutno 4 događaja stanja i 4 mreže veze koje su dobro. Odlučite da biste istražili problem, a zatim kliknite na **Sharepoint-Web** vezu mreže da biste saznali korijenu problem.  
  ![primjer vezu dobro mreže](./media/log-analytics-network-performance-monitor/npm-investigation02.png)

3. Na stranici naniže prikazuje sve veze subnetwork u **Sharepoint-Web** mrežnu vezu. Primijetit ćete da za oba subnetwork veze, latenciju sadrži su prijeđene prag čime se dobro mrežnu vezu. Možete pogledati i trendova Latencija veza subnetwork. Odabir vremena možete koristiti kontrolu u grafikonu usredotočili na potrebne vremenski raspon. Vidjet ćete doba dana kada kašnjenje je Pristigla njegov Vršna. Naknadno možete pretraživati zapisnike za ovaj vremensko razdoblje da biste istražili problem. Kliknite **Prikaz čvor veza** na naniže Dodatno.  
  ![primjer dobro podmreže veze](./media/log-analytics-network-performance-monitor/npm-investigation03.png)

4.  Slično kao na prethodnu stranicu, stranici naniže određeni subnetwork veze za popise dolje njene sastavnoj čvor veze. Možete izvoditi akcije slične ovdje kao u prethodnom koraku. Kliknite **Prikaz topologije** da biste pogledali topologije između čvorove 2.  
  ![primjer dobro čvor veze](./media/log-analytics-network-performance-monitor/npm-investigation04.png)

5. Svi putovi između 2 odabrani čvorovi se iscrtavaju na karti topologije. Možete vizualizirati topologije skoka po skoka usmjerava između dva čvorove na karti topologije. Pruža Očisti sliku koliko usmjerava postoji između dva čvorove i što putova izvodite podatkovne pakete. Grla performanse mreže su označeni crvenom bojom. Neispravni mrežne veze ili neispravan mrežnog uređaja možete pronaći tako da pogledate crvena boja elemenata na karti topologije.  
  ![primjer prikaza dobro topologije](./media/log-analytics-network-performance-monitor/npm-investigation05.png)

6. Gubitak, Latencija i broj putova u svaki put možete pregledati u oknu s **Detaljima put** . U ovom primjeru, vidjet ćete da postoje 3 dobro puta kao što je rečeno u oknu. Koristite na kliznik da biste pogledali detalje o tim dobro putovi.  Koristite potvrdne okvire da biste odabrali neki tako da se iscrtavaju topologije za samo jednu put. Vaš kotačića možete koristiti za povećavanje i smanjivanje karti topologije.

  U na sliku u nastavku jasno vidjet ćete korijenskog uzroka problema područja za određeni dio mreže tako da pogledate putovima i preskakanja crvenom bojom. Klikom na čvor na karti topologije otkriva svojstva čvor FQDN, uključujući i IP adrese. Klikom na u skoka prikazuje IP adresu na mjesto.  
  ![dobro topologija - put pojedinosti primjer](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="next-steps"></a>Daljnji koraci

- [Pretraživanje zapisnika](log-analytics-log-searches.md) radi prikaza detaljne mreže performanse podataka zapisa.
