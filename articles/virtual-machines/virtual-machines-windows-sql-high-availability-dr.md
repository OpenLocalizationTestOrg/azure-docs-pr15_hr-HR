<properties
    pageTitle="Visoke dostupnosti i oporavak Izrada za SQL Server | Microsoft Azure"
    description="Rasprave razne vrste HADR Strategije za SQL Server pokrenut virtualnim računalima sustava u Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Visoke dostupnosti i Izrada oporavak za SQL Server na virtualnim strojevima sa sustavom Azure

## <a name="overview"></a>Pregled

Virtualnim računalima sustava Microsoft Azure (VMs) sa sustavom SQL Server može pridonijeti donji trošak visoke dostupnosti i Izrada rješenje oporavak (HADR) baze podataka. Većina rješenja SQL Server HADR podržava Azure virtualnim strojevima samo za Azure i kao hibridnog rješenja. U rješenju samo za Azure cijelog sustava HADR pokreće se u Azure. U hibridnoj konfiguraciji dio rješenje pokreće se u Azure i na drugi dio pokreće lokalnog u tvrtki ili ustanovi. Fleksibilnost okruženje za Azure omogućuje vam da biste premjestili Azure potpuno ili djelomično zadovoljili proračuna i preduvjeti HADR svoje baze podataka sustava SQL Server.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="understanding-the-need-for-an-hadr-solution"></a>Objašnjenje potrebe za HADR rješenje

Preporučuje se najviše vama osigurava sustava baza podataka sadrži sadrže HADR mogućnosti koje je potrebno ugovor o razini usluge (SLA). Fact kojega Azure Mehanizmi visoke dostupnosti, kao što je servis automatskog popravka za servise u oblaku i otkrivanje oporavak pogreške za virtualnim strojevima ne samu jamči zadovoljavate željeni SLA. Ove mehanizme zaštititi visoke dostupnosti na VMs, ali ne visoke dostupnosti SQL Server izvodi unutar na VMs. Moguće je instancu sustava SQL Server nije uspjela tijekom na VM je na mreži i dobar. Nadalje, čak i visoke dostupnosti mehanizme nudi Azure omogućuju nedostupnost VMs zbog događaje kao što su oporavak putem softvera ili hardverske pogreške i nadogradnje operacijski sustav.

Osim toga, zemlj suvišnih prostora za pohranu (GRS) u Azure koje je primijenjeno sa značajkom pod nazivom zemlj replikacije, možda neće biti rješenje za oporavak odgovarajuće Izrada baze podataka. Budući da zemlj replikacije asinkrono šalje podatke, najnovija ažuriranja može biti izgubljene slučaju Izrada. Dodatne informacije o ograničenjima zemlj replikacije možete pronaći u odjeljku [zemlj replikacija nije podržana za podatke i datoteka zapisnika na zasebnom diskova](#geo-replication-support) .

## <a name="hadr-deployment-architectures"></a>Arhitekturi HADR implementacije

Tehnologije sustava SQL Server HADR koje su podržane u Azure obuhvaćaju sljedeće:

- [Stalna grupe dostupnosti](https://technet.microsoft.com/library/hh510230.aspx)
- [Baze podataka](https://technet.microsoft.com/library/ms189852.aspx)
- [Zapisnik dostave](https://technet.microsoft.com/library/ms187103.aspx)
- [Sigurnosno kopiranje i vraćanje sa servisom za spremište blobova platforme Azure](https://msdn.microsoft.com/library/jj919148.aspx)
- [Uvijek na instance prebacivanje klaster](https://technet.microsoft.com/library/ms189134.aspx)

Nije moguće kombinirati tehnologije zajedno za implementaciju sustava SQL Server rješenja koja ima visoke dostupnosti i Izrada oporavak mogućnosti. Ovisno o tehnologija koje koristite, hibridne implementacije možda ćete morati tunelom VPN-a s Azure virtualne mreže. U odjeljcima objašnjavaju neke implementacije arhitekturi primjer.

## <a name="azure-only-high-availability-solutions"></a>Samo za Azure: rješenja visoke dostupnosti

Možete imati rješenje visoke dostupnosti baze podataka sustava SQL Server u Azure pomoću uvijek na dostupnost grupe ili baze podataka zrcaljenja.

| Tehnologija                               | Primjer arhitekturi                    |
| ---------------------------------------- | ---------------------------------------- |
| **Stalna grupe dostupnosti**        | Sve dostupnost replike pokrenut u Azure VMs visoke dostupnosti unutar iste područja. Morate konfigurirati kontroler domene VM jer je domene servisa Active Directory potreban je Windows Server prebacivanje Klasteriranje (WSFC).<br/> ![Stalna grupe dostupnosti](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Dodatne informacije potražite u članku [Konfiguriranje uvijek na dostupnost grupe u Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Uvijek na instance prebacivanje klaster** | Instance klaster na prebacivanje (FCI) koji zahtijeva zajedničkog prostora za pohranu, možete stvoriti na različite načine 2.<br/><br/>1. FCI na dva čvor WSFC izvodi u Azure VMs s pohranom podržava klasteriranja rješenja drugih proizvođača. Određeni primjer koji koristi SIOS DataKeeper, potražite u članku [visoke dostupnosti za zajedničko korištenje datoteka pomoću WSFC i 3 proizvođača softvera SIOS Datakeeper](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>2. na FCI na dva čvor WSFC izvodi u Azure VMs s udaljenog iSCSI cilj zajedničko korištenje bloka za pohranu putem ExpressRoute. Na primjer, NetApp privatnih prostora za pohranu (NPS) izlaže odredište iSCSI putem ExpressRoute s Equinix za Azure VMs.<br/><br/>Treće strane zajedničkog prostora za pohranu i rješenja za replikaciju podataka, obratite se dobavljača probleme vezane uz pristup podacima na prebacivanje.<br/><br/>Imajte na umu da pomoću FCI pri vrhu [Azure pohrani](https://azure.microsoft.com/services/storage/files/) nije podržan, jer je to rješenje korištenje Premium prostora za pohranu. Radimo na to podržava uskoro. |

## <a name="azure-only-disaster-recovery-solutions"></a>Samo za Azure: Izrada oporavak rješenja

Možete imati Izrada oporavak rješenje za vaše baza podataka sustava SQL Server u Azure pomoću uvijek na grupe dostupnosti, baze podataka zrcaljenje ili sigurnosne kopije i vraćanje s blob-ova za pohranu.

| Tehnologija                               | Primjer arhitekturi                    |
| ---------------------------------------- | ---------------------------------------- |
| **Stalna grupe dostupnosti**        | Dostupnost replike izvodi preko više podatkovnim centrima u Azure VMs za oporavak Izrada. Rješenje regije-štiti od nedostupnosti dovršeno web-mjesta. <br/> ![Stalna grupe dostupnosti](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>Unutar područja, sve replike trebao bi biti unutar iste servis u oblaku i iste VNet. Budući da svako područje će imati zasebne VNet, ova rješenja ne mogu VNet VNet povezivanje. Dodatne informacije potražite u članku [Konfiguriranje VPN-a web-mjesto na portalu za Azure klasični](../vpn-gateway/vpn-gateway-site-to-site-create.md). |
| **Baze podataka**                   | Temeljnu glavnicu i zrcalne i poslužitelje s programom u različitim podatkovnim centrima za oporavak Izrada. Morate uvesti pomoću certifikata na poslužitelj jer je domene servisa active directory ne može obuhvaćati više podatkovnim centrima.<br/>![Baze podataka](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Sigurnosno kopiranje i vraćanje sa servisom za spremište blobova platforme Azure** | Baza podataka radnog izravno sa spremištem blobova u različitim podatkovnog centra za oporavak Izrada sigurnosne kopije.<br/>![Sigurnosno kopiranje i vraćanje](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Dodatne informacije potražite u članku [sigurnosnog kopiranja i vraćanja za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="hybrid-it-disaster-recovery-solutions"></a>Hibridno IT: Izrada oporavak rješenja

Možete imati rješenja za oporavak Izrada baze podataka sustava SQL Server u okruženju hibridnog IT pomoću uvijek na grupe dostupnosti, baze podataka, zapisnika dostavu i sigurnosno kopiranje i vraćanje s Azure blog prostora za pohranu.

| Tehnologija                               | Primjer arhitekturi                    |
| ---------------------------------------- | ---------------------------------------- |
| **Stalna grupe dostupnosti**        | Neke dostupnost replike izvodi u Azure VMs i druge replike radi lokalnog za oporavak Izrada web-mjesta. Web-mjesta radnog može biti bilo lokalno ili na Azure podatkovnog centra.<br/>![Stalna grupe dostupnosti](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Jer sve dostupnost replike mora biti u istoj WSFC klaster, klaster WSFC mora obuhvaćati i mreža (WSFC klaster više podmreže). Tu konfiguraciju zahtijeva VPN vezu između Azure i lokalne mreže.<br/><br/>Za oporavak uspješno Izrada od baza podataka, trebali biste instalirati i kontroler domene replike na web-mjestu za oporavak Izrada.<br/><br/>Nije moguće koristiti Čarobnjak za dodavanje replike u SSMS da biste dodali Azure replike postojeće uvijek na dostupnost grupe. Dodatne informacije potražite u članku vodič: proširivanje uvijek na dostupnost grupu za Azure. |
| **Baze podataka**                   | Jedan partnera za pokretanje programa Azure VM i druge pokretanje lokalnog za oporavak Izrada web-mjesta pomoću certifikata na poslužitelj. Partneri moraju biti iste domene servisa Active Directory i potreban je VPN veza.<br/>![Baze podataka](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Zrcaljenje scenarij drugu bazu podataka obuhvaća jednog partnerskog izvodi u programa Azure VM i na druge izvodi lokalno u istom domene servisa Active Directory za oporavak Izrada web-mjesta. Potreban je [VPN veza između Azure virtualne mreže i lokalne mreže](../vpn-gateway/vpn-gateway-site-to-site-create.md) .<br/><br/>Za oporavak uspješno Izrada od baza podataka, trebali biste instalirati i kontroler domene replike na web-mjestu za oporavak Izrada. |
| **Zapisnik dostave**                         | Jedan poslužitelju koji se izvodi u programa Azure VM i druge pokretanje lokalnog za oporavak Izrada web-mjesta. Zapisnik dostave ovisi o Windows zajedničko korištenje datoteka, tako da je potreban je VPN veza između Azure virtualne mreže i lokalne mreže.<br/>![Zapisnik dostave](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Za oporavak uspješno Izrada od baza podataka, trebali biste instalirati i kontroler domene replike na web-mjestu za oporavak Izrada. |
| **Sigurnosno kopiranje i vraćanje sa servisom za spremište blobova platforme Azure** | Lokalne baze podataka radnog izravno u spremište blobova platforme Azure za oporavak Izrada sigurnosne kopije.<br/>![Sigurnosno kopiranje i vraćanje](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Dodatne informacije potražite u članku [sigurnosnog kopiranja i vraćanja za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Važne napomene za SQL Server HADR u Azure

Azure VMs, prostora za pohranu i mreže imaju različite radu karakteristike od lokalnog, koje nisu virtualiziranom IT infrastrukturu. Potreban je uspješno implementacija rješenja HADR sustava SQL Server u Azure te razlikama i dizajniranje rješenje kako bi odgovarao ih.

### <a name="high-availability-nodes-in-an-availability-set"></a>Čvorovi visoke dostupnosti u skupu dostupnosti

Dostupnost skupova u Azure omogućuju smjestiti čvorove visoke dostupnosti u zasebne kvara domene (FDs) i ažuriranje domene (UDs). Za Azure VMs smjestiti u istom postavljanje dostupnosti, morate ih uvesti u istom servis u oblaku. Čvorovi u istoj oblaku može sudjelovati u isti skup dostupnost. Dodatne informacije potražite u članku [Upravljanje strojeva za dostupnost virtualne](virtual-machines-windows-manage-availability.md).

### <a name="wsfc-cluster-behavior-in-azure-networking"></a>Ponašanje klaster WSFC Azure s mrežom

Servis koji nisu-sa RFC standardima DHCP u Azure mogu prouzročiti stvaranja određene WSFC klaster konfiguracije uspjeti zbog naziv mreže klaster dodijeljene duplicirane IP adresu, kao što su istu IP adresu kao jedan od čvorove klaster. To je problem kada implementirate uvijek na dostupnost grupe, a to ovisi o značajku WSFC.

Zamislite kada dva čvor klaster je stvorili, a na mreži:

1. Klaster se priključuje na mrežu, a zatim NODE1 dinamički dodijeljena IP adresa za naziv mreže klaster zahtjeve za razgovore.

2. Nema IP adresa osim NODE1 na vlastitu IP adresa dobiva servis DHCP jer servis DHCP prepoznaje zahtjev potječe NODE1 sam.

3. Windows otkrije da dupliciranih adresa se dodjeljuju NODE1 i na naziv mreže klaster te zadanoj grupi klaster ne uspije prijavi.

4. U zadanu grupu klaster premješta NODE2 koje smatra NODE1 je IP adresa klaster IP adresa i objedinjuje zadane klaster grupe putem Interneta.

5. Kada NODE2 pokušava uspostaviti vezu s NODE1, pakete usmjereni na NODE1 nikad ostavite NODE2 jer razrješava NODE1 na IP adresu sa sobom. NODE2 ne može se uspostaviti vezu s NODE1, zatim gubi kvorum i zatvara klaster.

6. U aplikacijom NODE1 možete poslati pakete NODE2, ali ne može odgovoriti NODE2. NODE1 gubi kvorum i zatvara klaster.

Ovaj scenarij može se izbjegavati dodjeljivanjem Neiskorišteni statičke IP adresu, kao što je IP adresa link-local, kao što su 169.254.1.1, klaster naziv mreže da biste otvorili klaster mrežni naziv putem Interneta. Da biste pojednostavili postupak potražite u članku [Konfiguriranje klaster Windows prebacivanje u Azure za uvijek na dostupnost grupe](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Dodatne informacije potražite u članku [Konfiguriranje uvijek na dostupnost grupe u Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Podrška za ga slušatelj grupe dostupnosti

Dostupnost grupe slušače podržani su na Azure VMs izvodi Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 i Windows Server 2016. Tu podršku postala je moguće pomoću uravnoteženja krajnje točke omogućena na VMs Azure koji su čvorove dostupnost grupe. Posebne konfiguracije morate koraci za slušače tako da radi za oba klijentske aplikacije koji se koriste u Azure, kao i one se izvodi lokalno.

Postoje dvije glavne mogućnosti za postavljanje vašeg ga slušatelj: vanjski (javni) ni interne. Vanjski (javni) ga slušatelj koristi internetsku nasuprotne opterećenja i povezan s na javno virtualne IP (VIP) koji je dostupan putem Interneta. Interna ga slušatelj Interna opterećenja koji se koristi, a podržavaju samo klijenti unutar iste virtualne mreže. Za bilo koju vrstu opterećenja učitati, morate omogućiti Izravni poslužitelj vratiti. 

Ako grupe dostupnosti obuhvaća više Azure podmreže (kao što je implementacija koji obuhvaća Azure područja), morate uključiti niz za povezivanje klijent "**MultisubnetFailover = True**". Ovo rezultira pokušaja paralelno povezivanja na replike u različitim podmreže. Upute za postavljanje programa ga slušatelj potražite

- [Konfiguriranje programa ILB ga slušatelj za uvijek na dostupnost grupe u Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
- [Konfiguriranje vanjskog ga slušatelj za uvijek na dostupnost grupe u Azure](virtual-machines-windows-classic-ps-sql-ext-listener.md).

Možete i dalje se povezati svaki replike dostupnost zasebno povezivanjem izravno instanca servisa. Osim toga, jer su uvijek na dostupnost grupe kompatibilna s bazom podataka zrcaljenje klijenata, možete se povezati s replike dostupnost kao što je baza podataka zrcaljenje partnera pod uvjetom da na replike konfigurirane slične baze podataka:

- Jedan primarni replike i jedan sekundarnog replike

- Sekundarnog replike konfigurirana kao koje nisu čitljiv (**Čitljivi sekundarne** mogućnost postavljeno na **ne**)

Na primjer klijent niz za povezivanje koji odgovara tu konfiguraciju zrcaljenje nalik baze podataka pomoću servisa za ADO.NET ili nativni klijent za SQL Server je ispod:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Dodatne informacije o povezivanje s klijentom, potražite u članku:

- [Pomoću ključnih riječi niza veze za nativni klijent za SQL Server](https://msdn.microsoft.com/library/ms130822.aspx)
- [Klijenti s bazom podataka zrcaljenje sesiju (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
- [Povezivanje s ga Slušatelj grupe dostupnosti u hibridnog IT](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
- [Grupe dostupnosti slušače, povezivanje s klijentom i aplikacije prebacivanje (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
- [Baze podataka sustava nizove veze pomoću grupe dostupnosti](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Latenciju mreže u hibridnog IT

Trebali biste implementaciju rješenja HADR uz pretpostavku da možda biti vremenskog s visoke mrežom latencije između lokalne mreže i Azure razdoblja. Prilikom implementacije replike za Azure, trebali biste koristiti asinkronog potvrdi umjesto sinkrono potvrdi način rada za sinkronizaciju. Kada implementacija baze podataka zrcaljenje poslužitelje lokalnog i servisu Azure, koristite način visoke performanse umjesto načinu Visoki sigurnost.

### <a name="geo-replication-support"></a>Podrška za replikaciju za zemlj.

Zemlj replikacije u Azure diskova ne podržava podatkovne datoteke i datoteke zapisnika iste baze podataka za pohranu na zasebnom diskova. GRS replicira promjene na svakom disku neovisno i asinkrono. U ovom mehanizam jamčiti redoslijed pisanje unutar jedan disk na kopiji zemlj replicirati, ali ne svim zemlj replicirati kopije više diskova. Ako konfigurirate baze podataka za pohranu njegov podatkovne datoteke i njegove datoteke zapisnika na zasebnom diskova oporavljene diskova nakon na Izrada može sadržavati više ažurirane kopije datoteke podataka od datoteka zapisnika, čime se prekida dovršena prije pisanja prijava ACID svojstva transakcije i SQL Server. Ako ne postoji mogućnost da biste onemogućili zemlj replikacije na računu za pohranu, zadržati sve podatke i datoteka zapisnika za dani bazu podataka na istom disku. Ako morate koristiti više od jednog diska zbog veličine baze podataka, morate uvesti jednu od rješenja oporavak Izrada naveden da biste bili sigurni redundanciju podataka jer.

## <a name="next-steps"></a>Daljnji koraci

Ako je potrebno stvoriti Azure virtualnog računala sa sustavom SQL Server potražite u članku [Dodjeljivanje SQL Server virtualnog računala na Azure](virtual-machines-windows-portal-sql-server-provision.md).

Da biste ostvarili najbolje performanse iz sustava SQL Server na programa Azure VM, potražite upute u [Performanse najbolje prakse za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-performance.md).

Druge teme vezane uz izvodi SQL Server u Azure VMs, potražite u članku [SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Ostali resursi

- [Instaliranje novog skupa stabala Active Directory u Azure](../active-directory/active-directory-new-forest-virtual-machine.md)
- [Stvaranje WSFC klaster za uvijek na dostupnost grupe u Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)
