<properties
   pageTitle="Tehnički vodič: oporavak iz lokalne pogreške u Azure | Microsoft Azure"
   description="Članak na razumijevanje i dizajniranje prebacuju, Visoko dostupna, pogreške aplikacije, kao i planiranje Izrada oporavak filtriran na lokalne pogreške unutar Azure."
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

#<a name="azure-resiliency-technical-guidance-recovery-from-local-failures-in-azure"></a>Azure otpornost Tehnički vodič: oporavak iz lokalne pogreške u Azure

Postoje dvije primarni prijetnji dostupan aplikacije:

* Pogreška uređaja, kao što su pogona i poslužitelja
* Pojedinačno ključnih resurse, kao što su računalnim pod uvjetima Vršna učitavanja

Azure nudi kombinacije Upravljanje resursima, elasticity, opterećenja i particija da biste omogućili visoke dostupnosti u sljedećim okolnostima. Neke od tih značajki izvršavaju automatski za sve servise Azure. No u nekim slučajevima razvojni inženjer morate učiniti učiniti još nešto prednosti od njih.

##<a name="cloud-services"></a>Servisi u oblaku

Azure servise u Oblaku sastoji se od zbirke jednu ili više weba ili tempiranja uloga. Jedan ili više instanci uloge istovremeno možete pokrenuti. Konfiguracija određuje koliko je instanci. Uloga instance nadzirati i upravlja komponenta naziva kontrolerom tkanina. Kontrolerom tkanina otkriva i automatski odgovori softvera i hardvera pogreške.

Svaku instancu uloga pokreće se u vlastitom virtualnog računala (VM) i komunicira s njegova kontroler tkanina kroz agent za goste. Agent za goste prikuplja resursa i čvor metrika, uključujući VM korištenje, stanje, zapisnika, korištenje resursa, iznimke i uvjeta nije uspjelo. Kontrolerom tkanina upiti agent za goste konfigurirati intervalima i ponovnog pokretanja na VM ako agent za goste ne reagira. U slučaju pogreške hardver kontrolerom pridružene tkanina premješta sve instance problematične uloga novi čvor hardver i reconfigures mreže usmjeravanje prometa postoji.

Da biste im tih značajki, razvojni inženjeri potrebno je provjeriti da sve uloge servisa izbjegavajte pohranu stanje na instance uloge. Umjesto toga, sve stalni podatke trebali biste im pristupiti s durable prostor za pohranu, kao što su Azure prostora za pohranu ili baze podataka SQL Azure. Time se omogućuje sve uloge za rukovanje zahtjevima. Također znači da uloga instance možete prijeći prema dolje u bilo kojem trenutku bez stvaranja nedosljednosti u tranzitne ili stalni stanju servisa.

Zahtjev za pohranu stanja vanjsko uloge ima nekoliko posljedice. Znači, na primjer, koje sve promjene povezane tablice programa Azure prostora za pohranu treba promijeniti jednom transakcijom entitet grupe, ako je to moguće. Naravno, nije uvijek moguće napraviti sve promjene u jedan transakcije. Morate preuzeti posebnu pažnju da biste bili sigurni da uloga instance neuspjeha uzrokovati probleme kada su prekinuti dugoročnih postupaka koji obuhvaćaju dva ili više ažuriranja stalni stanje servisa. Ako neku drugu ulogu pokuša ponovno pokušajte kao što je postupak, trebali biste će se i rukovati slučaj kojem rad djelomično dovršena.

Na primjer, razmotrite servis particije podataka preko više trgovine. Ako ulogu suradnika funkcionira dok ga se relocating na shard, relocation na shard možda ne završi. Ili se relocation možda mogu ponoviti od njegova početka ulogom različite tempiranja, potencijalno uzrokuje bez nadređene podataka ili oštećenja podataka. Da biste spriječili probleme dugoročnih postupaka mora biti jedno ili oboje od sljedećeg:

 * *Idempotent*: kontrolirani bez popratne pojave. Da biste se idempotent, dugoročnih operacija treba imaju isti učinak bez obzira na to koliko je puta izvršava, čak i kada je prekinuta tijekom izvršavanja.
 * *Postupno restartable*: moći nastaviti s najnovije točke nije uspjelo. Da biste se postupno restartable, dugoročnih operacija treba sastoje se od slijeda manje atomske operacije. Je potrebno i zapis tijeku u durable prostor za pohranu, tako da svaki sljedeći poziva uzima gdje se zaustavio njegov drugi datum zbog međuovisnosti.

Na kraju, sve operacije dugoročnih treba moguće pozvati pritišćite tipku TAB dok se ne mogu biti uspješan. Ako, na primjer, operacije za dodjelu resursa bi mogli potvrdili Azure reda čekanja i zatim uklanjaju tempiranja ulogom samo kada to bude uspješno provedeno. Možda će biti potrebno očistiti podatke koje je prekinuta operacije stvaranje smeća.

###<a name="elasticity"></a>Elasticity

Početni broj instanci koje su pokrenute za svaku ulogu ovisi o konfiguraciji svaku ulogu. Administratori prethodno trebali biste konfigurirati svaku ulogu da biste pokrenuli s dva ili više instanci koji se temelji na očekivani Učitaj. Ali možete jednostavno skaliranje uloga instance prema gore ili dolje kao Upotreba uzoraka promjena. To možete učiniti ručno na portalu za Azure ili postupka možete automatizirati pomoću komponente Windows PowerShell, servis za upravljanje API ili Alati drugih proizvođača. Dodatne informacije potražite [u](../cloud-services/cloud-services-how-to-scale.md)članku automatsko skaliranje aplikacija.

###<a name="partitioning"></a>Particija

Kontrolerom Azure tkanina koristi dvije vrste particije:

* *Ažuriranje domene* koristi se za nadogradnju na servis instance uloga u grupama. Azure uvodi instanci servisa u više domena ažuriranja. Za nadogradnju na mjestu kontrolerom tkanina objedinjuje dolje sve instance u jedan ažuriranje domene, ažurira ih i pokreće ih prije prelaska sljedeće ažuriranje domene. Taj se način onemogućuje se dostupne tijekom postupka ažuriranja cijelu servisa.
* *Domene kvara* definira potencijalne točke hardver ili mreže nije uspjelo. Bilo koja uloga ima više instanci kontrolerom tkanina osigurava pojavljivanja su raspodijeliti više domena kvara, da biste spriječili ometa servisa Izolirani hardverske pogreške. Domena kvara upravljaju sve izlaganje poslužitelja i klaster pogreške.

[Azure ugovor o razini usluge (SLA)](https://azure.microsoft.com/support/legal/sla/) jamčiti da kada dva ili više instanci uloga web su implementiran na različitim kvara i nadogradnja domena, ćete imaju vanjskih povezivanje najmanje 99.95 iznimaka tome pravilu. Za razliku od ažuriranje domene ne postoji način da biste upravljali brojem kvara domene. Azure automatski dodjeljuje kvara domene i distribuira uloga instance preko njih. At najmanje prve dvije instance svaku ulogu spremaju se u različitim kvara i nadogradite domene da bi bilo koja uloga s barem dvije instance zadovoljavaju na SLA. Predstavljena je u sljedećem su dijagramu.

![Pojednostavnjeni prikaz odvajanja kvara domene](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-1.png)

###<a name="load-balancing"></a>Za ujednačavanje opterećenja

Dolazni promet web ulogu prolazi kroz bez praćenja stanja raspoređivača opterećenja koji distribuira zahtjevi klijenta među instance uloge. Uloge za pojedinačne instance imaju javnu IP adrese, a nisu izravno moguće adresirati s Interneta. Uloge web su bez praćenja stanja tako da se sve instance uloga moguće usmjeriti svaki zahtjev za klijenta. [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck.aspx) događaj podignut svakih 15 sekundi. Pomoću nje možete označava je li ulogu spremno za primanje promet ili hoće li je zauzet i treba uzeti iz raspoređivača opterećenja zakretanje.

##<a name="virtual-machines"></a>Virtualnim strojevima

Azure virtualnim strojevima razlikuje se od platforme kao što je servis (PaaS) za izračun ulogama u nekoliko običnoj odnosu visoke dostupnosti. U nekim slučajevima, to morate učiniti još nešto da biste bili sigurni visoke dostupnosti.

###<a name="disk-durability"></a>Rok trajanja na disku

Za razliku od PaaS uloga instance, podatke pohranjene na pogonima virtualnog računala je stalni čak i kad je je premještena virtualnog računala. Azure virtualnim strojevima VM diskova koji postoje upotrijebili blob-ova u Azure prostora za pohranu. Zbog osobina dostupnost Azure prostora za pohranu podataka spremljenih na pogonima virtualnog računala dostupan je i Visoko.

Imajte na umu da pogonu D (u sustavu Windows VMs) iznimku to pravilo. Pogonu D je zapravo fizičke prostora za pohranu na poslužitelju za bicikle koji hostira na VM i njegove podatke izgubit će se ako je na VM brisanja. Pogonu D namijenjen samo privremeno spremište. U Linux, Azure "obično" (ali ne uvijek) izlaže lokalni disk privremene kao /dev/sdb blok uređaj. To je često postavljen tako da Azure Linux Agent kao /mnt/resource ili /mnt postavljanja točaka (može konfigurirati putem /etc/waagent.conf).

###<a name="partitioning"></a>Particija

Azure nativno razumije razine u aplikaciji PaaS (web uloge i uloge suradnika) i tako može ispravno preraspodijeliti na kvara i ažuriranje domene. Nasuprot tome, razine u do infrastrukture kao aplikacije servisa (IaaS) moraju se ručno definirati putem dostupnosti skupa. Dostupnost skupovi su potrebni za programa SLA u odjeljku IaaS.

![Dostupnost skupovi za Azure virtualnim strojevima](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-2.png)

U prethodnom dijagramu sloju Internet Information Services (IIS) (koji funkcionira kao sloj web app) i sloju SQL (koji funkcionira kao podataka sloju) dodjeljuju se na dostupnost različite skupove. Time se osigurava da sve instance svaki sloju imaju zalihosti hardver raspodijeliti virtualnim strojevima svim kvara domenama, a da cijele razine ne uzimaju se prema dolje tijekom ažuriranje.

###<a name="load-balancing"></a>Za ujednačavanje opterećenja

Ako na VMs mora imati promet raspodijeliti ih, morate grupirati VMs u do aplikacija i učitavanje saldo preko određene TCP i UDP krajnje. Dodatne informacije potražite u članku [opterećenja virtualnih računala](../virtual-machines/virtual-machines-linux-load-balance.md). Ako na VMs dobivate unos iz drugog izvora (na primjer, stavljanja mehanizam), raspoređivača opterećenja nije potrebna. Raspoređivača opterećenja koristi provjera osnovni stanja da biste odredili hoće li promet moraju se poslati u čvor. Također je moguće stvoriti vlastiti probes implementaciju metriku stanja specifičnim aplikacijama koje određuju hoće li se VM trebale primiti promet.

##<a name="storage"></a>Prostor za pohranu

Azure prostora za pohranu je osnovne durable podataka servisa Azure. Pruža blob, tablice, reda čekanja i pohranu VM disk. Koristi kombinaciju replikacije i upravljanje resursima kako visoke dostupnosti unutar jednog podatkovnog centra. Dostupnost za pohranu Azure SLA jamčiti koje barem 99.9 iznimaka tome pravilu:

* Pravilno oblikovani zahtjeve za dodavanje, ažuriranje, čitanje, i brisanje podataka obradit će se uspješno i ispravno.
* Računi za pohranu će imati veza s Internetom pristupnika.

###<a name="replication"></a>Replikacije

Azure prostora za pohranu olakšava rok trajanja podataka čuvanjem više kopija svih podataka na drugi pogona preko podsustava potpuno neovisno fizičke prostora za pohranu unutar područja. Podaci sinkronizirano replicirati, a sve kopije predaju prije pisanja je označeni. Azure prostora za pohranu je svakako dosljedan, što znači da se čitanja zajamčena u skladu s vizualnim najnovije zapisivanja. Osim toga, kopije podataka neprestano pregledavanja za otkrivanje i popravak bitne rot, često zanemaren prijetnju integritet pohranjene podatke.

Usluge pogodnost replikacije samo pomoću Azure prostora za pohranu. Programiranje servisa ne morate učiniti još nešto se može oporaviti iz lokalne pogreške.

###<a name="resource-management"></a>Upravljanje resursima

Prostor za pohranu račune stvorene nakon svibnja 2014., možete povećati da najviše 500 TB (prethodne Maksimalna je 200 TB). Ako je potreban dodatan prostor, aplikacija biti osmišljeni tako da pomoću više računa za pohranu.

###<a name="virtual-machine-disks"></a>Diskova virtualnog računala

Virtualni pogon diska pohranjuju se kao blob stranice u spremište Azure mu Dodijeli iste rok trajanja i skalabilnost svojstva kao spremište blobova platforme. Ovaj dizajn čini podatke na disku virtualnog računala stalni, čak i ako poslužitelj koji se izvodi na VM ne uspije, a na VM mora se ponovno pokrenuti na neki drugi poslužitelj.

##<a name="database"></a>Baze podataka

###<a name="sql-database"></a>SQL baze podataka

Baze podataka SQL Azure sadrži bazu podataka kao usluga. Omogućuje aplikacije da biste brzo Dodjela resursa, umetnite podatke i relacijske baze podataka u upitu. Nudi mnoge poznate značajke sustava SQL Server i funkcionalnosti, tijekom abstracting teret hardver, konfiguracije, zakrpa i otpornost.

>[AZURE.NOTE] Baze podataka SQL Azure sadrže slične značajke ikone značajka sa sustavom SQL Server. Namijenjen je za ispunjavanje drugačiji skup preduvjeti – koji sadrži jedinstveno prilagođene aplikacije oblaka (elastic mjerilo, bazu podataka kao servis za smanjivanje troškova održavanja i tako dalje). Dodatne informacije potražite u članku [Odaberite oblak mogućnost SQL Server: baze podataka SQL Azure (PaaS) ili SQL Server na Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

####<a name="replication"></a>Replikacije

Baze podataka SQL Azure nudi ugrađene otpornost na razini čvora nije uspjelo. Sve zapisivanja u bazu podataka automatski se replicirati na dva ili više pozadine čvorove kroz postupak za izvršavanje kvorum. (Primarnih osnovnih stranica i barem jedan sekundarni morate potvrditi da aktivnost napisali zapisnik transakcija prije transakcije se smatra uspjelo i vraća.) U slučaju pogreške čvor baza podataka automatski ne uspijeva putem na neki od sekundarnog replike. Zbog toga pružanju tranzitne veze za klijentske aplikacije. Zbog toga sve klijente baze podataka SQL Azure morate provesti neki oblik rukovanje tranzitne veze. Dodatne informacije potražite u članku [ponovno pokušajte određene smjernice za servis](../best-practices-retry-service-specific.md).

####<a name="resource-management"></a>Upravljanje resursima

Svaki baze podataka pri stvaranju je konfiguriran za korištenje gornjem ograničenja. Trenutno dostupno Maksimalna veličina je 1 TB (veličina razlikuju se ograničenja na temelju servisa sloju, potražite u članku [razine usluge i razine performansi baze podataka SQL Azure](../sql-database/sql-database-resource-limits.md#service-tiers-and-performance-levels). Kada bazi podataka dodirne ograničenje veličine gornjem, odbacuje dodatne naredbe za umetanje ili ažuriranje. (Slanje upita i brisanje podataka nije uvijek moguće.)

Baze podataka, baze podataka SQL Azure koristi u tkanina za upravljanje resursima. Međutim, umjesto kontroler tkanina koristi Nazovi topologije da biste otkrili pogreške. Svaki replike klasteru ima dvije susjeda i odgovoran za otkrivanje kada ih otvorite prema dolje. Kada funkcionira kopiju, njegov susjeda pokretanje ponovno konfiguriranje agent da biste ponovno stvorili na nekom drugom računalu. Ograničavanje modul navedeni su da biste bili sigurni logičke poslužitelj ne koristite previše resursa na računalo ili premašuju ograničenja za fizičke na računalu.

###<a name="elasticity"></a>Elasticity

Ako aplikacija potrebno više od 1 TB ograničenje baze podataka, morate provesti skaliranje iz pristup. Promijenite li odgovor s bazom podataka SQL Azure tako da ručno particija poznat i kao sharding, podataka preko više SQL baza podataka. Takvog skaliranje iz nudi prilike da biste postigli gotovo linearni trošak growth s ljestvicom. Elastic rasta ili kapaciteta na zahtjev rast možete s rastuće troškove po potrebi jer su naplatiti baze podataka na temelju prosjeka stvarne veličine koristi dnevno, ne temelji na Maksimalna moguća veličina.

##<a name="sql-server-on-virtual-machines"></a>SQL Server na virtualnim računalima

Ako instalirate virtualnim računalima sustava na Azure SQL Server (verzija 2014 ili novija verzija), možete iskoristiti tradicionalni dostupnost značajki sustava SQL Server. Te značajke obuhvaćaju grupe dostupnosti AlwaysOn i baze podataka. Imajte na umu da Azure VMs, za pohranu i povezivanje s mrežom imaju različite radu karakteristike od lokalnog, koje nisu virtualiziranom IT infrastrukturu. Potreban je uspješno implementaciji sustava visoke dostupnosti/Izrada oporavak rješenje (HA na DR) SQL Server Azure te razlikama i dizajniranje rješenje kako bi odgovarao ih.

###<a name="high-availability-nodes-in-an-availability-set"></a>Čvorovi visoku dostupnost u skupu dostupnosti

Kada implementirate visoku dostupnost rješenja u Azure, pomoću dostupnost u Azure smjestiti čvorove visoku dostupnost u zasebnom kvara domene i nadogradnje domene. Samo da razjasnimo postavljanje dostupnosti je Azure pojam. Je najbolji način koje morate slijediti da biste bili sigurni da vaših baza podataka dostupni uistinu iznimno, bez obzira koristite grupe dostupnosti AlwaysOn, baze podataka ili nešto drugo. Ako ne prođete najbolja praksa, možda ćete u odjeljku false pretpostavci da vaš sustav iznimno dostupna je. No u stvari, vaše čvorove sve uspijeva istodobno jer se zbivaju smjestiti u istu domenu kvara u području Azure.

Ove preporuke nije primjenjivo, s dostave zapisnika. Kao značajka oporavak Izrada, potrebno je provjeriti poslužitelji koristite u zasebnom Azure regijama. Definicija, te područja su zasebnom kvara domene.

Za Azure oblaka Services VMs implementiran putem klasične portal u isti skup dostupnost, morate ih uvesti u istom servis u Oblaku. VMs implementiran putem Azure Voditelj resursa (trenutni portal) ne morate to ograničenje. Za klasični portal implementiran VMs u Oblaku Azure, samo čvorove u istom servis u Oblaku može sudjelovati u isti skup dostupnost. Uz to, VMs Services oblaka mora biti u istoj virtualne mreže da biste bili sigurni da su održavanje njihove IP-ovi čak i nakon servisa automatskog popravka. Taj se način izbjegavaju to izbjeglo ažuriranje DNS-a.

###<a name="azure-only-high-availability-solutions"></a>Samo za Azure: visoku dostupnost rješenja

Možete odrediti visoku dostupnost rješenja za baze podataka sustava SQL Server u Azure pomoću grupe dostupnosti AlwaysOn ili baze podataka.

Na sljedećem su dijagramu pokazuje arhitektura grupe dostupnosti AlwaysOn izvodi na virtualnim strojevima sa sustavom Azure. Ovaj dijagram preuzeta iz iscrpan članak o toj temi [visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim strojevima sa sustavom Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![Grupe dostupnosti AlwaysOn u Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-1.png)

Možete i automatski Dodjela resursa za grupe dostupnosti AlwaysOn implementacije završetka do kraja na Azure VMs pomoću predloška AlwaysOn na portalu za Azure. Dodatne informacije potražite u članku [SQL Server AlwaysOn koja nudi u galeriji Portal za Microsoft Azure](https://blogs.technet.microsoft.com/dataplatforminsider/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery/).

Na sljedećem su dijagramu pokazuje korištenje baze podataka sustava virtualnim računalima sustava na Azure. Također je snimljena iz detaljnije tema [visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim strojevima sa sustavom Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

![Zrcaljenje baze podataka u programu Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-2.png)

>[AZURE.NOTE] Oba arhitekturi zahtijevaju kontroler domene. Međutim, s baze podataka sustava, moguće je za korištenje certifikata server da biste uklonili potrebe za kontroler domene.

##<a name="other-azure-platform-services"></a>Druge usluge platforme Azure

Aplikacije koje se temelji na Azure prednosti platformu mogućnosti da biste vratili iz lokalne pogreške. U nekim slučajevima može potrajati određene akcije da biste povećali dostupnost određene scenariju.

###<a name="service-bus"></a>Servis Bus

Da biste smanjili protiv privremene nedostupnosti od Bus servisa Azure, razmislite o stvaranju durable klijentsko red. To privremeno koristi mehanizam za zamjenski, lokalni prostora za pohranu za spremanje poruka koje nije moguće dodati red Bus servisa. Aplikaciju možete odlučiti kako rukovati privremeno spremljene poruke kada je servis je vraćen. Dodatne informacije potražite u članku [najbolje prakse za korištenje servisa Bus poboljšanja performansi brokered poruka](../service-bus-messaging/service-bus-performance-improvements.md) i [Usluge Bus (Izrada oporavak)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services).

###<a name="hdinsight"></a>HDInsight

Prema zadanim postavkama u spremište blobova platforme Azure pohrane podataka koji je pridružen Azure HDInsight. Azure prostora za pohranu navodi svojstva visoku dostupnost i rok trajanja spremište blobova platforme. Obrada više čvor povezane sa zadacima Hadoop MapReduce pojavljuje se na na tranzitne Hadoop Distributed datoteka sustava (HDFS) koja vam je dodijeljena kada HDInsight potreban. Rezultati iz MapReduce posao spremaju se po zadanom u spremište blobova platforme Azure tako da obrađeni podaci durable i ostaje iznimno dostupne nakon klaster Hadoop se uklanjaju resursi. Dodatne informacije potražite u članku [HDInsight (Izrada oporavak)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services).

##<a name="checklists-for-local-failures"></a>Kontrolni popisi za lokalne pogreške

###<a name="cloud-services"></a>Servisi u oblaku

  1. Pročitajte odjeljak servise u Oblaku ovog dokumenta.
  2. Konfiguriranje barem dvije instance za svaku ulogu.
  3. Zadržava stanje durable prostor za pohranu, ne pojave ulogu.
  4. Pravilno rukovati StatusCheck događaj.
  5. Da biste prelomili povezane promjene u transakcije kada je to moguće.
  6. Provjerite jesu li zadaci ulogu suradnika idempotent i restartable.
  7. I dalje pozivanje operacije dok se ne mogu biti uspješan.
  8. Razmislite o autoscaling strategije.

###<a name="virtual-machines"></a>Virtualnim strojevima

  1. Pročitajte odjeljak virtualnim strojevima ovog dokumenta.
  2. Korištenje pogonu D za stalnih prostor za pohranu.
  3. Grupa strojeva u sloju servisa u skupu dostupnost.
  4. Konfiguriranje uravnoteženja opterećenja i neobavezna probes.

###<a name="storage"></a>Prostor za pohranu

  1. Pročitajte odjeljak za pohranu ovog dokumenta.
  2. Koristite više prostora za pohranu računa kada podataka ili propusnost premašuje kvote.

###<a name="sql-database"></a>SQL baze podataka

  1. Pročitajte odjeljak baze podataka SQL ovog dokumenta.
  2. Implementacija pravila Ponovi rukovanja pogreškama tranzitne.
  3. Korištenje particija/sharding kao strategije skaliranje izlaz.

###<a name="sql-server-on-virtual-machines"></a>SQL Server na virtualnim računalima

  1. Pregledajte SQL Server na virtualnim strojevima odjeljak ovog dokumenta.
  2. Slijedite prethodno preporuke za virtualnih računala.
  3. Pomoću značajke visoke dostupnosti SQL Server, kao što su AlwaysOn.

###<a name="service-bus"></a>Servis Bus

  1. Pročitajte odjeljak Bus servisa ovog dokumenta.
  2. Razmislite o stvaranju durable reda čekanja klijentsko kao sigurnosnu kopiju.

###<a name="hdinsight"></a>HDInsight

  1. Pročitajte odjeljak HDInsight ovog dokumenta.
  2. Bez dodatnih dostupnost koraci su potrebni za lokalne pogreške.

##<a name="next-steps"></a>Daljnji koraci

Ovaj je članak dio niza filtriran na [Azure otpornost Tehnički vodič](./resiliency-technical-guidance.md). Sljedeći članak u ovoj seriji je [oporavak iz područja razini usluge prekidu](./resiliency-technical-guidance-recovery-loss-azure-region.md).
