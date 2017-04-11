<properties
   pageTitle="Otpornost za oporavak od gubitka Azure regija Tehnički vodič | Microsoft Azure"
   description="Članak na objašnjenje i dizajniranje pogreške aplikacije kvara prebacuju, Visoko dostupna, kao i planiranje Izrada oporavak"
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

#<a name="azure-resiliency-technical-guidance-recovery-from-a-region-wide-service-disruption"></a>Azure otpornost Tehnički vodič: oporavak iz područja razini usluge prekidu

Azure podijeljen fizički i logički jedinice naziva područja. Područje se sastoji od jednog ili više podatkovnim centrima uz. Prilikom pisanja ovog Azure ima dvadeset i četiri područja svijeta.

Rijetko okolnostima moguće je u cijelo područje može postati nedostupne, na primjer zbog pogrešaka u mreži. Ili funkcije izgubiti potpuno, na primjer zbog prirodnim Izrada. U ovom se odjeljku objašnjavaju mogućnosti Azure za stvaranje aplikacije koje su raspodijeliti područja. Takve raspodjele pomaže da biste minimizirali mogućnost pogreška u jednom području može utjecati drugim regijama.

##<a name="cloud-services"></a>Servisi u oblaku

###<a name="resource-management"></a>Upravljanje resursima

Računalnim instance možete raspodijelite područja stvaranjem zasebnom oblaku na svakom ciljnom području i objavljivanje paketa za implementaciju svaki servis u oblaku. Međutim, imajte na umu da distribucija promet na servise u oblaku u različitim područjima mora biti implementirano razvojni inženjer ili sa servisa za upravljanje promet.

Određivanje koliko je instanci rezervnih uloge za implementaciju unaprijed za oporavak Izrada je važno aspekt planiranje kapaciteta. Imate pune sekundarne implementacije osigurava kapaciteta već dostupne po potrebi; No to učinkovito doubles trošak. Uobičajeni obrazac tako da imate mali, sekundarne implementacije samo dovoljna da biste pokrenuli ključnih services. Ovaj mali sekundarne implementacije dobra je ideja, oba rezervirati kapaciteta, a za testiranje konfiguracije sekundarne okruženje.

>[AZURE.NOTE]Kvota za pretplatu nije jamstvo kapaciteta. Kvote je jednostavno ograničenje kreditne kartice. Da bi kapaciteta, potrebno je definirati potreban broj uloge u modelu servisa, a mora biti implementirano uloge.

###<a name="load-balancing"></a>Za ujednačavanje opterećenja

Da biste učitali saldo promet preko područja potreban je rješenje za upravljanje promet. Azure nudi [Upravitelj promet Azure](https://azure.microsoft.com/services/traffic-manager/). Također možete iskoristiti prednost servisa drugih proizvođača koji omogućuju slične promet mogućnosti upravljanja.

###<a name="strategies"></a>Strategije

Mnoge druge strategije dostupni su za implementaciju raspodijeljeno računalnim preko područja. To morate prilagoditi određeni poslovni preduvjeti i okolnostima aplikacije. Visoke razine na pristupa može se podijeliti u sljedećih kategorija:

  * __Ponovno implementirate na Izrada__: U takvog aplikacija je ponovno implementirati ispočetka trenutku Izrada. To je prikladno za rješavaju aplikacije koje zahtijevaju sigurno oporavku.

  * __Toplo rezervnih (Aktivno/pasivni)__: sekundarne servis je stvoren u alternativni regija i uloge uvode se da bi minimalnog kapaciteta. Međutim, uloge ne primite radnog promet. Taj se način je korisno za aplikacije koje nije namijenjena da biste promet raspodijelite područja.

  * __Tipkovni rezervnih (Aktivno/aktivno)__: aplikacija namijenjen je primanje radnih opterećenja na više područja. Servisa u oblaku na svakom području može se konfigurirati tako kapaciteta veća od potrebne za oporavak Izrada. Osim toga, možda skaliranje servisa u oblaku u trenutku pojavljivanja Izrada i prebacivanje Odjava prema potrebi. Taj se način zahtijeva znatno ulaganja u dizajnu aplikacije, ali ima brojne prednosti. To obuhvaća slabe i sigurno oporavku, neprekinuti testiranje svih mjesta oporavak i učinkovito korištenje kapaciteta.

Potpune raspodijeljeno dizajna je izvan opsega ovog dokumenta. Dodatne informacije potražite u članku [oporavak Izrada i visoke dostupnosti za Azure aplikacije](https://aka.ms/drtechguide).

##<a name="virtual-machines"></a>Virtualnim strojevima

Oporavak infrastrukture kao usluge (IaaS) virtualnim strojevima (VMs) je slična platforme kao što je oporavak na mnogo običnoj za izračun usluge (PaaS). Postoje važne razlike, no blokirale u zbog činjenice u kojoj je IaaS VM sastoji se od na VM i VM disk.

  * __Sigurnosno kopiranje Azure koristi za stvaranje sigurnosne kopije unakrsni regije koje su aplikacije dosljedni__.
  [Sigurnosno kopiranje Azure](https://azure.microsoft.com/services/backup/) omogućuje klijentima da biste stvorili dosljedan kopija aplikacije na više diskova VM i podržava replikacije sigurnosne kopije preko područja. To možete učiniti tako da zemlj replicirati sigurnosno kopiranje zbirke ključeva prilikom stvaranja. Imajte na umu da replikacija sigurnosno kopiranje zbirke ključeva mora biti konfigurirano u vrijeme stvaranja. To nije moguće postaviti kasnije. Ako je područje izgubiti, Microsoft će postati sigurnosnih kopija dostupna korisnicima. Klijenti bit će moći vratiti u bilo koji od njihove točke konfigurirani vraćanja.

  * __Razdvajanje podatkovni disk s diska operacijski sustav__. Važna činjenica za IaaS VMs je nije moguće promijeniti disk operacijski sustav bez ponovno stvoriti u VM. To nije problem ako strategije oporavak implementirati nakon Izrada. Međutim, ako koristite Toplo rezervnih pristup rezervirati kapaciteta možda je problem. Da biste implementirate to ispravno, morate imati ispravne operacijski sustav disk implementiran na primarnih i sekundarnih mjesta, a podataka aplikacije mora biti pohranjena na zasebnom disk. Ako je moguće, koristite konfiguracije standardne operacijski sustav koji se može pružati na oba mjesta. Nakon prebacivanje, zatim priložite pogon podataka u postojeće IaaS VMs u sekundarni Kontroler. Koristite AzCopy da biste kopirali snimke diskove podataka na udaljenom mjestu.

  * __Imajte na umu potencijalne probleme dosljednost nakon na prebacivanje zemlj više VM diskova__. VM diskova primjenjuju kao BLOB polja za pohranu Azure i imati isti karakteristikama zemlj replikacije. Osim ako se koristi [Azure sigurnosne kopije](https://azure.microsoft.com/services/backup/) , postoje bez jamstva dosljednost preko diskova, budući da zemlj replikacije asinkronog i replicira neovisno. Pojedinačne VM diskova su zajamčeno neočekivanim zatvaranjem programa dosljedan stanja nakon prebacivanje zemlj., ali ne dosljedan preko diskova. To može uzrokovati probleme u nekim slučajevima (na primjer, u slučaju Podjela disk).

##<a name="storage"></a>Prostor za pohranu

###<a name="recovery-by-using-geo-redundant-storage-of-blob-table-queue-and-vm-disk-storage"></a>Oporavak pomoću zemlj suvišnih prostora za pohranu blob, tablice, reda čekanja i pohranu VM disk

U Azure, blob-ova, tablice, redovima i VM diskova su sve zemlj. – replicirati prema zadanim postavkama. To se naziva kao zemlj suvišnih prostora za pohranu (GRS). GRS replicira prostora za pohranu podataka u paru Standard stotine milja udaljenosti unutar određenog regiji. Navedite dodatnu rok trajanja u slučaju da postoji glavne podatkovnog centra Izrada namijenjen je GRS. Microsoft kontrola kada se pojavi prebacivanje i prebacivanje ograničeno je na glavnu disasters u kojem na izvorno mjesto primarni se smatra nepopravljiva u pametnije vremenskog razdoblja. U nekim slučajevima to može biti nekoliko dana. Podataka obično je replicirati za nekoliko minuta, iako interval sinkronizacije još nije prekriveno skupovima ugovor o razini usluge.

U slučaju na zemlj. – prebacivanje će biti ne mijenja način na koji je račun pristupa (tipku URL i račun neće se promijeniti). Račun za pohranu, no bit će u nekoj drugoj regiji nakon prebacivanje. To može utjecati na programe koji zahtijevaju regionalne afinitet s računa za pohranu. Čak i za servise i aplikacijama koje je potreban je račun za pohranu u istom podatkovnog centra, unakrsno Standard Latencija i troškove propusnosti može biti privlačnim vizualizacije razloga za privremeno premjestiti promet s područjem prebacivanje. Faktor nije u cjelokupan Izrada Strategije za oporavak.

Uz automatsko prebacivanje nudi GRS, Azure je uvedena servis koji vam omogućuje pristup čitanju kopiju podataka na mjestu sekundarne prostora za pohranu. To se naziva za pohranu zemlj suvišnih pristup za čitanje (RA GRS).

Dodatne informacije o GRS i RA GRS prostora za pohranu potražite u članku [replikacije Azure prostora za pohranu](../storage/storage-redundancy.md).

###<a name="geo-replication-region-mappings"></a>Mapiranja regija zemlj replikacije:

Važno je da znate gdje se podaci zemlj. – replicirati, da biste znali gdje želite implementirati instance podataka koje je potrebno regionalne afinitet s prostora za pohranu. Sljedeća tablica prikazuje uparivanja primarnih i sekundarnih mjesto:

[AZURE.INCLUDE [paired-region-list](../../includes/paired-region-list.md)]

###<a name="geo-replication-pricing"></a>Zemlj replikacije cijene:

Zemlj replikacije je uključen u trenutnom cijene za pohranu Azure. To se naziva zemlj suvišnih prostora za pohranu (GRS). Ako ne želite podataka zemlj replicirati možete onemogućiti zemlj replikacije za vaš račun. Ta mogućnost naziva lokalno suvišnih prostora za pohranu i on se naplaćuje po eskontiranu cijeni u usporedbi s GRS.

###<a name="determining-if-a-geo-failover-has-occurred"></a>Određivanje prebacivanje zemlj pojavila

Ako se pojavi prebacivanje zemlj., to će objavljena na [Nadzorna ploča za stanje servisa Azure](https://azure.microsoft.com/status/). Aplikacije možete implementirati automatiziranog srednje vrijednosti, no otkrivanje prateći zemlj područja za svoj račun za pohranu. To se može koristiti za pokretanje ostalih operacija oporavak, kao što su aktivacija računalnim resursa u zemlj. – regije koje premjestiti njihov prostora za pohranu. Upit za to možete izvršiti iz Upravljanje servisom API-JA, pomoću [Se svojstva računa za pohranu](https://msdn.microsoft.com/library/ee460802.aspx). Odgovarajući svojstva su:

    <GeoPrimaryRegion>primary-region</GeoPrimaryRegion>
    <StatusOfPrimary>[Available|Unavailable]</StatusOfPrimary>
    <LastGeoFailoverTime>DateTime</LastGeoFailoverTime>
    <GeoSecondaryRegion>secondary-region</GeoSecondaryRegion>
    <StatusOfSecondary>[Available|Unavailable]</StatusOfSecondary>

###<a name="vm-disks-and-geo-failover"></a>VM diskova i prebacivanje za zemlj.

Kako je opisano u odjeljku na VM diskova postoje bez jamstva dosljednost podataka na VM diskova nakon na prebacivanje. Da biste bili sigurni ispravnosti sigurnosne kopije, sigurnosne kopije proizvoda kao što su zaštita upravitelja u podacima moraju se sigurnosno kopiranje i vraćanje podataka aplikacije.

##<a name="database"></a>Baze podataka

###<a name="sql-database"></a>SQL baze podataka

Baze podataka SQL Azure omogućuje dvije vrste oporavak: zemlj vraćanja i aktivni replikacije zemlj..

####<a name="geo-restore"></a>Vraćanje za zemlj.

[Zemlj vraćanja](../sql-database/sql-database-recovery-using-backups.md#geo-restore) nije dostupan s bazama podataka Basic, standardna i Premium. Pruža mogućnost oporavka zadanu bazu podataka nije dostupan zbog incident u području koje se hostira baze podataka. Slično da biste vratili točke u vrijeme, zemlj vraćanja ovisi sigurnosne kopije baze podataka u zemlj suvišnih Azure prostora za pohranu. Vraća iz zemlj replicirati sigurnosne kopije i stoga je prebacuju na kvarove prostora za pohranu u području primarni. Dodatne informacije potražite u članku [Vraćanje baze podataka SQL Azure ili prebacivanje na sekundarni](../sql-database/sql-database-disaster-recovery.md).

####<a name="active-geo-replication"></a>Aktivni replikacije zemlj.

[Aktivni replikacije zemlj](../sql-database/sql-database-geo-replication-overview.md) dostupna je za sve razine baze podataka. Služi za aplikacije koje tretiraju redovno oporavak ne nude zemlj vraćanja. Koristite Active replikacije zemlj., možete stvoriti do četiri čitljiv secondaries na poslužiteljima u različitim područjima. Prebacivanje na neku razinu na secondaries možete pokrenuti. Osim toga, aktivni replikacije zemlj mogu se podržava aplikacija nadogradnju ili relocation scenarije, kao i uravnoteženje opterećenja za radnih opterećenja samo za čitanje. Dodatne informacije potražite u članku [Konfiguriranje zemlj replikacije](../sql-database/sql-database-geo-replication-portal.md) i uvoza [putem sekundarne bazu podataka](../sql-database/sql-database-geo-replication-failover-portal.md). Pogledajte [Dizajn aplikaciju za oporavak Izrada oblak pomoću aktivni zemlj. – replikacije u SQL baze podataka](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md) i [Upravljanje vodoravnim nadogradnje aplikacije oblaka korištenju SQL replikacije baze podataka Active zemlj. –](../sql-database/sql-database-manage-application-rolling-upgrade.md) detalje o načinu dizajna i implementacija aplikacije i nadogradnje aplikacije bez nedostupnost.

###<a name="sql-server-on-virtual-machines"></a>SQL Server na virtualnim računalima

Različite mogućnosti dostupne su za oporavak i visoke dostupnosti SQL Server 2012 (i noviji) radi virtualnim računalima sustava u Azure. Dodatne informacije potražite u članku [visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim strojevima sa sustavom Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

##<a name="other-azure-platform-services"></a>Druge usluge platforme Azure

Kada se pokuša pokrenuti servis u oblaku u više Azure regija, morate uzeti u obzir posljedice za svaku od vašeg ovisnosti. U sljedećim se odjeljcima smjernice specifične za servis pretpostavlja da morate koristiti istu servisa Azure u alternativni Azure Standard. To uključuje konfiguracija i podataka replikacije zadatke.

>[AZURE.NOTE]U nekim slučajevima korake može pomoći u smanjenju specifične za servis nedostupnosti umjesto događaja cijelu podatkovnog centra. Iz perspektive aplikacije servisa specifične nedostupnosti možda samo kao ograničavanje i zahtijeva privremeno migracije servis zamjenski Azure regiju.

###<a name="service-bus"></a>Servis Bus

Azure Bus servis koristi jedinstveni prostor za naziv koji obuhvaćaju Azure područja. Dakle obavezne prvi je postavljanje prostori bus potrebne servisa u području zamjenski. No postoje i zahtjevi za rok u redu čekanja poruka. Postoji nekoliko Strategije za replikaciju poruke preko Azure područja. Detalje o te replikacije strategije i druge Strategije oporavka Izrada, potražite u članku [najbolje prakse za insulating aplikacije izvoditi na servis Bus kvarove i disasters](../service-bus-messaging/service-bus-outages-disasters.md). Ostala razmatranja dostupnost potražite u članku [Servis Bus (dostupnost)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="app-service"></a>Aplikacije servisa

Da biste migrirali aplikacije servisa za Azure aplikacijama, kao što je web-aplikacijama ili aplikacije Mobile sekundarne Azure regiju, morate imati sigurnosnu kopiju web-mjesta za objavljivanje. Ako se prekida obuhvaćaju cijeli Azure podatkovnog centra, možda je moguće koristiti FTP da biste preuzeli nedavne sigurnosne kopije sadržaja web-mjesta. Zatim stvorite novu aplikaciju u području zamjenski osim u slučaju da prije toga rezervirati kapaciteta. Objavljivanje web-mjesta na novo područje, a promjene potrebnih konfiguracija. Te promjene mogu uključivati nizove veze baze podataka ili drugih postavki namijenjena određenoj regiji. Ako je potrebno, dodavanje SSL certifikata na web-mjesta i promijenite DNS CNAME zapis tako da se naziv prilagođene domene upućuje na redeployed Azure web-aplikacije URL.

###<a name="hdinsight"></a>HDInsight

Podaci koji su povezani s HDInsight pohranjuju se po zadanom u spremište blobova platforme Azure. HDInsight zahtijeva da Hadoop klaster MapReduce poslove moraju Suradnja nalaziti na istom području kao račun za pohranu koji sadrži podatke analiziraju. Ako koristite značajku zemlj replikacije dostupan prostor za pohranu Azure, možete pristupiti podatke u području sekundarne gdje je replicirati podatke ako iz nekog razloga primarni regija više nije dostupan. Možete stvoriti novu Hadoop klaster u regiji gdje je je replicirati podaci i nastaviti obradu. Ostala razmatranja dostupnost potražite u članku [HDInsight (dostupnost)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="sql-reporting"></a>Izvješćivanje o pogreškama SQL

Trenutačno oporavak od gubitka područja za Azure zahtijeva više instanci SQL izvješćivanje u različitim područjima Azure. Ove instance SQL Reporting trebate pristupiti iste podatke, a te podatke trebali biste dobiti vlastitu oporavak planiranje slučaju na Izrada. Možete održavati i vanjske sigurnosne kopije RDL datoteka za svako izvješće.

###<a name="media-services"></a>Media Services.

Azure Media Services ima različite oporavak pristup za kodiranje i strujanje. Strujanje je obično više ključnih tijekom regionalne prekida. Priprema za to, imat ćete računa za servise za medijske sadržaje u dvije različite regije Azure. Šifrirana sadržaj treba nalaziti u obje regije. Tijekom pogreške, možete preusmjeriti strujanje promet zamjenski regiju. Kodiranje moguće je izvesti u bilo kojem Azure regiji. Ako je kodiranje osjetljive na vrijeme, na primjer tijekom obrade uživo događaj, mora biti spremni za slanje poslove zamjenski podatkovnim centrom tijekom pogreške.

###<a name="virtual-network"></a>Virtualne mreže

Datoteka konfiguracije pružaju najbrži način da biste postavili virtualne mreže u alternativni Azure regiji. Nakon konfiguriranja virtualne mreže u primarni Azure regiji, [Izvoz postavki virtualne mreže](../virtual-network/virtual-networks-create-vnet-classic-portal.md) za trenutni mrežu na mreži konfiguracijska datoteka. U slučaju prekida u primarni regiji, [Vraćanje virtualne mreže](../virtual-network/virtual-networks-create-vnet-classic-portal.md) iz spremljene konfiguracijskoj datoteci. Zatim konfigurirajte ostale servise u oblaku, virtualnim strojevima ili više lokacija postavke za rad s novi virtualne mreže.

##<a name="checklists-for-disaster-recovery"></a>Kontrolni popisi za oporavak Izrada

##<a name="cloud-services-checklist"></a>Kontrolni popis za usluge oblaka

  1. Pročitajte odjeljak servise u Oblaku ovog dokumenta.
  2. Stvaranje Strategije za oporavak Izrada više područja.
  3. Razumijevanje gubitke u rezervirati kapacitet u alternativni područja.
  4. Koristite promet usmjeravanje Alati, kao što su Azure promet Upravitelj.

##<a name="virtual-machines-checklist"></a>Kontrolni popis za virtualnim strojevima

  1. Pročitajte odjeljak virtualnim strojevima ovog dokumenta.
  2. Pomoću [Azure sigurnosnog kopiranja](https://azure.microsoft.com/services/backup/) možete stvoriti aplikaciju dosljedan sigurnosne kopije preko područja.

##<a name="storage-checklist"></a>Kontrolni popis za pohranu

  1. Pročitajte odjeljak za pohranu ovog dokumenta.
  2. Nemojte ga onemogućivati zemlj replikacije resursa za pohranu.
  3. Razumijevanje zamjenski područja za zemlj replikaciju u slučaju prebacivanje.
  4. Stvorite prilagođene sigurnosne kopije strategije korisnički kontrolirano prebacivanje strategije.

##<a name="sql-database-checklist"></a>Kontrolni popis za SQL baze podataka

  1. Pročitajte odjeljak baze podataka SQL ovog dokumenta.
  2. Pomoću [Zemlj vraćanja](../sql-database/sql-database-recovery-using-backups.md#geo-restore) ili [Zemlj replikacije](../sql-database/sql-database-geo-replication-overview.md) po potrebi.

##<a name="sql-server-on-virtual-machines-checklist"></a>SQL Server na virtualnim strojevima kontrolni popis

  1. Pregledajte SQL Server na virtualnim strojevima odjeljak ovog dokumenta.
  2. Koristite grupe dostupnosti AlwaysOn regije- ili baze podataka.
  3. Umjesto pomoću sigurnosno kopiranje i vraćanje bloba prostora za pohranu.

##<a name="service-bus-checklist"></a>Kontrolni popis za servis Bus

  1. Pročitajte odjeljak Bus servisa ovog dokumenta.
  2. Konfiguriranje servisa Bus prostor naziva u alternativni regija.
  3. Razmislite o prilagođene replikacije strategije poruke preko područja.

##<a name="app-service-checklist"></a>Kontrolni popis za aplikacije servisa

  1. Pročitajte odjeljak aplikacije servisa za ovog dokumenta.
  2. Održavanje sigurnosne kopije web-mjesta izvan primarni regija.
  3. Ako je nedostupnosti djelomične pokušati dohvatiti trenutnog web-mjesta s FTP.
  4. Plan za implementaciju web-mjesta u novu ili postojeću web-mjesta u alternativni regiji.
  5. Planiranje promjene konfiguracije za aplikacije i DNS CNAME zapisa.

##<a name="hdinsight-checklist"></a>Kontrolni popis za HDInsight

  1. Pročitajte odjeljak HDInsight ovog dokumenta.
  2. Stvorite novi Hadoop klaster u regiji repliciranu podacima.

##<a name="sql-reporting-checklist"></a>Kontrolni popis za izvješćivanje SQL

  1. Pročitajte odjeljak izvješćivanje SQL ovog dokumenta.
  2. Održavanje zamjenski instance SQL izvješćivanje u nekoj drugoj regiji.
  3. Imaju zasebna plan za replikaciju cilj da biste to područje.

##<a name="media-services-checklist"></a>Kontrolni popis za Media Services

  1. Pročitajte odjeljak Media Services ovog dokumenta.
  2. Stvaranje računa za servise za medijske sadržaje u alternativni regija.
  3. Kodiranje isti sadržaj u obje regije za podršku strujanje prebacivanje.
  4. Slanje kodiranja poslove zamjenski područje slučaju servisa prekidu.

##<a name="virtual-network-checklist"></a>Kontrolni popis za virtualne mreže

  1. Pročitajte odjeljak virtualne mreže ovog dokumenta.
  2. Korištenje izvozi postavke virtualne mreže da biste ponovno stvorili u drugoj regiji.

##<a name="next-steps"></a>Daljnji koraci

Ovaj je članak dio niza filtriran na [Azure otpornost Tehnički vodič](./resiliency-technical-guidance.md). Sljedeći članak u ovoj seriji usredotočuje se na [oporavak iz lokalnog podatkovnog centra za Azure](./resiliency-technical-guidance-recovery-on-premises-azure.md).
