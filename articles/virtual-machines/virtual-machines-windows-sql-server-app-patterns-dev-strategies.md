<properties
    pageTitle="SQL Server aplikacije uzorci na VMs | Microsoft Azure"
    description="U ovom se članku opisuje uzoraka aplikacije za SQL Server na Azure VMs. Nudi rješenje projektantima i razvojni inženjeri temelj za arhitektura dobar aplikacije i dizajna."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="luisherring"
    manager="jhubbard"
    editor=""
    tags="azure-service-management,azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="lvargas" />

# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Uzorci aplikacije i razvoja Strategije za SQL Server na virtualnim računalima za Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="summary"></a>Sažetak:
Određivanje koje aplikacije uzorak ili uzorke koje želite koristiti za aplikacije sustava SQL Server utemeljen na servisu Azure okruženje je je važno dizajn odluka i bit će potrebno pune razumijevanja SQL Server i sve komponente infrastrukture Azure funkcioniranje. Pomoću servisa Azure Infrastruktura sustava SQL Server, možete jednostavno migrirati, održavati i nadzirati postojećih aplikacija sustava SQL Server ugrađen u Windows Server na virtualnim strojevima u Azure.

Cilj ovog članka je pružanje projektantima rješenja i razvojni inženjeri temelj za arhitektura dobar aplikacije i dizajn, mogu slijediti prilikom migracije postojeće aplikacije Azure kao i razvoj aplikacija za nove u Azure.

Za svaki uzorak aplikacije pronaći ćete na lokalni scenarij, njegov odgovarajući Oblačić s omogućenim rješenje i povezane Tehničke preporuke. Osim toga, u članku govori o specifične za Azure razvoja strategije tako da se aplikacija možete dizajnirati pravilno. Zbog mnogo uzoraka moguće aplikacije preporučuje se da projektantima i razvojnim inženjerima trebali biste odabrati najprikladnije uzorak svoje aplikacije i korisnicima.

**Tehnička suradnici:** Luis Carlos Vargas Herring, Madhan Arumugam Ramakrishnan

**Tehnička pregledavatelji:** Corey Sanders, Drew McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani, Schackow isto Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Uvod

Više vrsta aplikacija n sloja razviti tako što komponente slojeve drugu aplikaciju na različitim računalima i kao zasebnu komponente. Na primjer, možete postaviti klijentska aplikacija i tvrtke pravila komponente u jednom računalu, pristupne web razina i sloju komponente access podataka u nekom drugom računalu i sloju pozadinske baze podataka u nekom drugom računalu. Ta vrsta strukturiranja olakšava izdvajanja svaki sloju međusobno. Ako promijenite Odakle dolaze podaci, ne morate promijeniti klijenta ili web-aplikacije, ali samo podataka programa access sloju komponente.

Uobičajeni *n sloja* aplikacije sadrži sloju prezentacije, sloju tvrtke i razina podataka:


| Razina              | Opis                                                                                                                                                                     |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Prezentacije** | *Prezentacija sloju* (web razina, pristupne sloju) je sloj u kojima korisnici upotrebljavaju aplikacije.                                                                      |
| **Tvrtke**     | *Tvrtke sloju* (Srednji sloj) je sloj koji sloju prezentaciju i razina podataka koristite za komunikaciju i sadrži osnovne funkcije sustava. |
| **Podataka**         | *Razina podataka* je server u kojoj su pohranjeni podaci aplikacije (na primjer, poslužitelj izvodi SQL Server).                                                             |

Slojevi aplikacije opisuju logička grupiranja funkcionalnost i komponenti u aplikaciji; dok razine opisuju fizičke raspodjelu funkcionalnosti i komponente na zasebnom fizičke poslužitelja, računala, mreže ili udaljenog mjesta. Slojevi aplikacija može se nalaziti na istom fizičke računalu (isti sloju) ili distribuirati zasebnom računala (n-sloju) i komponente svaki sloju komunicirati s komponente u drugi Slojevi kroz dobro definiranom sučelja. Možete shvatiti sloju termina kao upućivanje fizičke raspodjele uzoraka kao što su dvije sloja, tri sloja, a n sloja. **Uzorak 2 sloja aplikacije** sadrži dvije razine aplikacije: poslužitelj aplikacije i poslužitelj baze podataka. Događa direktnu komunikaciju između poslužitelja aplikacija i poslužitelj baze podataka. Poslužitelj aplikacije sadrži komponente web razina i tvrtke sloja. U **aplikaciji sloja 3 uzorka**, postoje tri razine aplikacije: web-poslužitelj, poslužitelj aplikacije koja sadrži poslovne logike sloju i/ili poslovne sloju podataka programa access komponente, i poslužitelj baze podataka. Komunikacije između web-poslužitelj i poslužitelj baze podataka se ne događa putem poslužitelja aplikacija. Detaljne informacije o web-mjesto slojeve aplikacije i u okvir za razine potražite u članku [Vodič arhitektura aplikacije Microsoft](https://msdn.microsoft.com/library/ff650706.aspx).

Prije početka pročitajte ovaj članak, imat ćete znanja na osnovne koncepte SQL Server i Azure. Informacije potražite u članku [SQL Server knjige na mreži](https://msdn.microsoft.com/library/bb545450.aspx), [SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md) i [Azure.com](https://azure.microsoft.com/).

U ovom se članku opisuje nekoliko uzoraka aplikacije koje se mogu prikladna za jednostavne aplikacija, kao i vrlo složene aplikacijama. Prije dovršenog svaki uzorak, preporučujemo da ste upoznati s servise za pohranu podataka dostupni servisu Azure, kao što su [Azure prostora za pohranu](../storage/storage-introduction.md), [Baze podataka SQL Azure](../sql-database/sql-database-technical-overview.md)i [SQL Server u programa Azure virtualnog računala](virtual-machines-windows-sql-server-iaas-overview.md). Da biste najbolje odluke dizajna za aplikacije, razumijevanje kada koristiti jasno koji servis za pohranu podataka.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Odaberite SQL Server Azure virtualnog računala kada:

- Potreban vam je kontrolu na SQL Server i Windows. Ako, na primjer, to mogu sadržavati verzija programa SQL Server, posebne hitne, konfiguracije performanse, itd.

- Potrebna potpuna kompatibilnost sustava SQL Server lokalnog sustava, a želite premjestiti postojeće aplikacije za Azure kao-je.

- Želite odražava mogućnosti Azure okruženja, ali baze podataka SQL Azure ne podržava sve značajke koje je potrebno aplikacije. To može obuhvaćati sljedećih područja:

    - **Veličina baze podataka**: trenutku ažuriran u ovom se članku baze podataka SQL podržava baze podataka do 1 TB podataka. Ako aplikacija zahtijeva više od 1 TB podataka, a ne želite implementirati prilagođeni sharding rješenja, preporučuje se da koristite SQL Server u programa Azure virtualnog računala. Najnovije informacije potražite u članku [Skaliranje izvan Azure SQL baza podataka](https://msdn.microsoft.com/library/azure/dn495641.aspx) i [Azure SQL baze podataka usluge razine i performanse razine](../sql-database/sql-database-service-tiers.md).
    - **Usklađenost HIPAA**: zdravstvene Kupci i proizvođačima softvera (ISV) mogu odlučiti za [SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md) umjesto [Baze podataka SQL Azure](../sql-database/sql-database-technical-overview.md) jer je SQL Server u programa Azure virtualnog računala opisana po HIPAA tvrtke pridružiti ugovor (BAA). Informacije o usklađenosti potražite u članku [Microsoft Azure Trust Center: usklađenost](https://azure.microsoft.com/support/trust-center/compliance/).
    - **Značajke instanci razini**: u isto vrijeme SQL baze podataka ne podržava značajke koje live izvan baze podataka (kao što su povezani poslužitelji Agent zadaci, svojstvo FileStream, broker servisa sustava itd.). Dodatne informacije potražite u članku [smjernice za baze podataka SQL Azure i ograničenja](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>1 sloja (jednostavno): jedan virtualnog računala

U ovaj uzorak aplikacije implementacije svoju aplikaciju za SQL Server i baze podataka samostalne virtualnog računala u Azure. Isti virtualnog računala sadrži klijenta/web-aplikacije, poslovne komponente, sloj pristupa podacima i poslužitelj baze podataka. Prezentacije, tvrtke i podataka pristupnu šifru logično odvojene, ali fizički smješten u jednom poslužitelju računala. Većina kupaca počinju ovaj uzorak aplikacije i nakon toga ih Skaliraj izvan dodavanjem više uloge web ili virtualnim strojevima njihove sustav.

Ovaj uzorak aplikacija je korisno kada:

- Želite jednostavan Migracija Azure platformu za procjenu li platforme odgovore preduvjeti za aplikaciju sustava ili ne.

- Želite li zadržati sve razine aplikacije smješten u istom virtualnog računala u istom Azure podatkovnog centra u da biste smanjili latencije između razine.

- Želite li brzo Dodjela resursa za razvoj i testiranje okruženja za kratki vremenskog razdoblja.

- Koju želite izvesti opterećenjem testiranje za različite razine radno opterećenje, ali u isto vrijeme ne želite vlasnik i održavanje mnogo fizičke strojeva cijelo vrijeme.

Na sljedećem su dijagramu pokazuje jednostavan lokalnog scenarij i uvođenje njegov oblaka omogućena rješenja u jednom virtualnog računala u Azure.

![Uzorak aplikacije 1 sloja](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Implementacija sloj tvrtke (poslovne logike i podataka pristupili komponentama) na isti fizičke sloju kao sloj prezentaciju možete poboljšajte performanse aplikacije, osim ako ih ne morate koristiti odvojene sloju zbog skalabilnost ili sigurnosne opasnosti.

Budući da je to vrlo uobičajeni uzorak započeti, vam mogli korisni sljedeći članak na migracije za premještanje podataka sustava SQL Server VM: [Migracija baze podataka SQL Server na programa Azure VM](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3 sloja (jednostavno): više virtualnim strojevima

U ovaj uzorak aplikacije implementirati aplikaciju sloja 3 u Azure potvrđivanjem svaki razina aplikacije u drugu virtualnog računala. To okruženje fleksibilne jednostavno skaliranje gore i skaliranje iz scenarijima za. Kada jedan virtualnog računala sadrži klijenta/web-aplikacije, drugu hostira poslovne komponente, a drugu hostira poslužitelj baze podataka.

Ovaj uzorak aplikacija je korisno kada:

- Želite li Migracija složene aplikacija za baze podataka za Azure virtualnih računala.

- Želite da se aplikacija za različite razine da biste se nalaziti u različitim područjima. Na primjer, možda zajedničkom korištenju baze podataka koje su implementiran na više područja za izvješćivanja.

- Koju želite premjestiti aplikacijama iz lokalnog virtualiziranom platforme za Azure virtualnih računala. Detaljne rasprave na aplikacijama potražite u članku [što je Enterprise aplikaciju](https://msdn.microsoft.com/library/aa267045.aspx).

- Želite li brzo Dodjela resursa za razvoj i testiranje okruženja za kratki vremenskog razdoblja.

- Koju želite izvesti opterećenjem testiranje za različite razine radno opterećenje, ali u isto vrijeme ne želite vlasnik i održavanje mnogo fizičke strojeva cijelo vrijeme.

Sljedeći dijagram prikazuje kako možete postaviti jednostavne 3 sloja aplikacije u Azure potvrđivanjem svaki razina aplikacije u drugu virtualnog računala.

![aplikacija sloja 3 uzorka](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

U ovaj uzorak računala postoji samo jedan virtualnog računala (VM) svaki sloju. Ako imate više VMs u Azure, preporučujemo da postavite virtualne mreže. [Virtualne mreže Azure](../virtual-network/virtual-networks-overview.md) stvara granicu pouzdano sigurnost i omogućuje VMs za komunikaciju među same privatne IP adrese. Osim toga, uvijek provjerite da sve internetske veze samo otvorite prezentaciju sloju. Kada slijedite ovaj uzorak aplikacije, upravljanje mreže sigurnosne grupe pravila za kontrolu pristupa. Dodatne informacije potražite u članku [Dopusti vanjski pristup vašem VM pomoću portala za Azure](virtual-machines-windows-nsg-quickstart-portal.md).

Na dijagramu internetskih protokola može biti TCP, UDP, HTTP ili HTTPS.

>[AZURE.NOTE] Postavljanje virtualne mreže u Azure je besplatna. Međutim, vam se naplatiti za pristupnik za VPN koja se povezuje s lokalnim. Trošak temelji se na vrijeme koje dodijeljenu i dostupna je veza.

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>sloja 2 i 3 sloja s prezentacijom sloju skaliranje Izlaz

U ovaj uzorak aplikacije implementaciju aplikacija za bazu podataka sloja 2 ili 3 sloja virtualnim računalima sustava za Azure potvrđivanjem svaki razina aplikacije u drugu virtualnog računala. Osim toga, skaliranje out sloju prezentacije zbog povećana količinu zahtjevi za klijenta.

Ovaj uzorak aplikacija je korisno kada:

- Koju želite premjestiti aplikacijama iz lokalnog virtualiziranom platforme za Azure virtualnih računala.

- Želite li skaliranje out sloju prezentacije zbog povećana količinu zahtjevi za klijenta.

- Želite li brzo Dodjela resursa za razvoj i testiranje okruženja za kratki vremenskog razdoblja.

- Koju želite izvesti opterećenjem testiranje za različite razine radno opterećenje, ali u isto vrijeme ne želite vlasnik i održavanje mnogo fizičke strojeva cijelo vrijeme.

- Želite li vlasnik okruženju infrastrukture koje se mogu mijenjati veličinu prema gore ili dolje na zahtjev.

Sljedeći dijagram prikazuje kako možete smjestiti razine aplikacije u više virtualnim strojevima Azure po skaliranje out sloju prezentacije zbog povećana količinu zahtjevi za klijenta. Kao što se vidi u dijagramu, odgovoran za raspodjela promet preko više virtualnim strojevima i i određivanje koji web-poslužitelj za povezivanje s je Azure raspoređivača opterećenja. Imate više instanci web-poslužiteljima iza raspoređivača opterećenja osigurava visoke dostupnosti sloju prezentacije.

![Uzorak aplikacije - prezentacije sloju skaliranje izgleda](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Najbolje prakse za sloja 2, 3 sloju ili n sloja uzoraka koji imaju više VMs u jedan sloju

Preporučuje se položite virtualnim strojevima koji pripadaju isti sloju u istom servis u oblaku, a u istom dostupnost postavljanje. Na primjer, potvrdite skup web-poslužiteljima **CloudService1** i **AvailabilitySet1** i postavljanje poslužitelja baze podataka u **CloudService2** i **AvailabilitySet2**. Raspoloživost postavljanje u Azure omogućuje vam smjestiti čvorove visoke dostupnosti u zasebnom kvara domene i nadogradnja domene.

Korištenje više instanci VM sloj, morate konfigurirati Azure opterećenja između aplikacija razine. Da biste konfigurirali raspoređivača opterećenja u svakom sloju, stvaranje uravnoteženja krajnje točke na svakom sloju VMs zasebno. Za određene sloju, prvo stvorite VMs u istom servis u oblaku. Na taj način da imaju isti javno virtualne IP adresa. Nakon toga stvorite krajnje točke na jedan od virtualnim strojevima na tom sloju. Zatim da isti krajnje točke dodijeliti druge virtualnim strojevima na tom sloju za opterećenja. Stvaranjem skupu uravnoteženja promet raspodijelite više virtualnim strojevima i omogućuju raspoređivača opterećenja da biste utvrdili koji čvor povezati ako ne uspije čvor VM pozadinskog. Na primjer, imate više instanci web-poslužiteljima iza raspoređivača opterećenja osigurava visoke dostupnosti sloju prezentacije.

Kao preporučenim načinom rada uvijek pripazite da sve internetske veze najprije morate otvoriti u sloju prezentacije. Prezentacija sloj pristupa sloju tvrtke, a zatim tvrtke razina pristupa sloju podataka. Dodatne informacije o tome da biste omogućili pristup sloju prezentacije potražite u članku [Dopusti vanjski pristup vašem VM pomoću portala za Azure](virtual-machines-windows-nsg-quickstart-portal.md).

Imajte na umu da funkcionira raspoređivača opterećenja servisu Azure slično učitavanje balancers u okruženju na lokaciji. Dodatne informacije potražite u članku [opterećenja za servise Azure infrastrukture](virtual-machines-windows-load-balance.md).

Osim toga, preporučujemo da ste postavili privatna mreža za virtualnim strojevima pomoću Azure virtualne mreže. Time se omogućuje komunikaciju među same putem privatne IP adrese. Dodatne informacije potražite u članku [Azure virtualne mreže](../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>sloja 2 i 3 sloja sa business sloju skaliranje Izlaz

U ovaj uzorak aplikacije implementaciju aplikacija za bazu podataka sloja 2 ili 3 sloja virtualnim računalima sustava za Azure potvrđivanjem svaki razina aplikacije u drugu virtualnog računala. Osim toga, možda ćete morati distribuiranje aplikacija komponente poslužitelja za više virtualnim strojevima zbog složenost aplikacije.

Ovaj uzorak aplikacija je korisno kada:

- Koju želite premjestiti aplikacijama iz lokalnog virtualiziranom platforme za Azure virtualnih računala.

- Želite distribuirati aplikaciju komponente poslužitelja za više virtualnim strojevima zbog složenost aplikacije.

- Koju želite premjestiti poslovne logike Tamni lokalnog aplikacije (redak specifični za poslovanje) LOB virtualnim računalima sustava za Azure. LOB aplikacije se skup ključnih aplikacijama koje su ključni za pokretanje tvrtke, kao što su računovodstvo, ljudskih resursa (HR), lijepljenje, upravljanja lancem nabave i planiranja aplikacije resursa.

- Želite li brzo Dodjela resursa za razvoj i testiranje okruženja za kratki vremenskog razdoblja.

- Koju želite izvesti opterećenjem testiranje za različite razine radno opterećenje, ali u isto vrijeme ne želite vlasnik i održavanje mnogo fizičke strojeva cijelo vrijeme.

- Želite li vlasnik okruženju infrastrukture koje se mogu mijenjati veličinu prema gore ili dolje na zahtjev.

Na sljedećem su dijagramu pokazuje scenarij u lokalni i njegov oblaka omogućena rješenja. U ovom scenariju, postavite razine aplikacije na više virtualnim računalima sustava u Azure po skaliranje izvan tvrtke sloj koji sadrži poslovne logike sloju i komponenti podataka programa access. Kao što se vidi u dijagramu, odgovoran za raspodjela promet preko više virtualnim strojevima i i određivanje koji web-poslužitelj za povezivanje s je Azure raspoređivača opterećenja. Imate više instanci poslužitelja aplikacije iza raspoređivača opterećenja osigurava visoke dostupnosti sloju tvrtke. Dodatne informacije potražite u članku [najbolje prakse za sloja 2, 3 sloju ili uzoraka n sloja aplikacije koji imaju više virtualnim strojevima u jedan sloju](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![Uzorak aplikacije s tvrtke sloju skalom izgleda](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>sloja 2 i 3 sloja s prezentacije i tvrtke razine skaliranje iz te HADR

U ovaj uzorak aplikacije implementaciju aplikacija za bazu podataka sloja 2 ili 3 sloja virtualnim računalima sustava za Azure raspodijeliti sloju prezentacije (web-poslužitelj) i poslovne sloju komponente (poslužitelj aplikacije) da biste više virtualnim strojevima. Osim toga, implementirati visoku dostupnost i Izrada rješenja za oporavak baze podataka na Azure virtualnim računalima sustava.

Ovaj uzorak aplikacija je korisno kada:

- Koju želite premjestiti aplikacijama virtualiziranom platforme lokalnim Azure implementacijom mogućnosti za oporavak Izrada i SQL Server visoke dostupnosti.

- Želite li skaliranje sloju prezentacije i sloju tvrtke zbog povećana količinu zahtjevi za klijenta i složenost aplikacije.

- Želite li brzo Dodjela resursa za razvoj i testiranje okruženja za kratki vremenskog razdoblja.

- Koju želite izvesti opterećenjem testiranje za različite razine radno opterećenje, ali u isto vrijeme ne želite vlasnik i održavanje mnogo fizičke strojeva cijelo vrijeme.

- Želite li vlasnik okruženju infrastrukture koje se mogu mijenjati veličinu prema gore ili dolje na zahtjev.

Na sljedećem su dijagramu pokazuje scenarij u lokalni i njegov oblaka omogućena rješenja. U ovom scenariju skaliranje sloju prezentacije i poslovne komponente sloju na više virtualnim računalima sustava u Azure. Osim toga, implementirati visoke dostupnosti i Izrada oporavak (HADR) tehnike za baze podataka sustava SQL Server u Azure.

Pokretanje više kopija aplikacije u drugu VMs Provjerite jeste li zahtjeve preko njih za ujednačavanje opterećenja li. Ako imate više virtualnim strojevima, koje morate donijeti sve VMs jesu li dostupni i pokrenut na jednom mjestu u vremenu. Ako konfigurirate opterećenja, Azure opterećenja prati stanje VMs i usmjerava dolazni pozivi čvorove dobar funkcionira VM pravilno. Informacije o postavljanju opterećenja virtualnih računala potražite u članku [uravnoteženje opterećenja za servise Azure infrastrukture](virtual-machines-windows-load-balance.md). Imate više instanci poslužitelja web- a aplikacije iza raspoređivača opterećenja osigurava visoke dostupnosti razine prezentacije i tvrtke.

![Skaliranje Izlaz i visoka dostupnost](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Najbolje prakse za aplikaciju uzorke koje je obavezna SQL HADR

Prilikom postavljanja sustava SQL Server visoke dostupnosti i rješenja za oporavak Izrada virtualnim računalima sustava u Azure postavljanje virtualne mreže za virtualnim računalima sustava pomoću [Azure virtualne mreže](../virtual-network/virtual-networks-overview.md) je obavezna.  Virtualnim strojevima unutar virtualne mreže čak i nakon servisa nedostupnost će imati stabilan privatne IP adrese, tako izbjeći ažuriranje vremena potrebnog za razrješavanje DNS naziva. Osim toga, virtualne mreže omogućuje vam da biste proširili lokalne mreže za Azure i stvara granicu pouzdanih sigurnost. Ako, na primjer, ako je vaša aplikacija ograničenja korporacijsku domenu (primjerice, provjera autentičnosti sustava Windows, servisa Active Directory), postavljanje [Azure virtualne mreže](../virtual-network/virtual-networks-overview.md) je potrebno.

Većina korisnika koji su pokrenuti kod proizvodnje na Azure, su zadržavanjem primarnih i sekundarnih replike Azure.

Informacije i vodiči za visoke dostupnosti i Izrada oporavak tehnike, potražite u članku [visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>sloja 2 i 3 sloja pomoću Azure VMs i servise u Oblaku

U ovaj uzorak aplikacije implementaciju sloja 2 ili 3 sloja aplikacija za Azure pomoću oba [Servisa u Oblaku Azure](../cloud-services/cloud-services-choose-me.md#tellmecs) (web- a tempiranja uloge - platforme kao Service (PaaS)) i [virtualnim računalima sustava Azure](virtual-machines-windows-about.md) (Infrastruktura kao Service (IaaS)). Korištenje [Azure servise u Oblaku](https://azure.microsoft.com/documentation/services/cloud-services/) za sloju sloju/tvrtke prezentacije i SQL Server na [virtualnim računalima sustava Azure](virtual-machines-windows-about.md) za sloju podataka je korisno za većinu aplikacija koje se izvode na Azure. Razlog je da imate računalnim instancu izvode na servise u Oblaku nudi programa jednostavno upravljanje, implementaciju, nadzor i skaliranje izlaz.

Servisi u Oblaku, Azure održava infrastrukturu za vas, izvodi redovno održavanje, zakrpa operacijski sustavi i pokušava oporaviti od servisa i hardver pogrešaka. Kada aplikacija mora skaliranje Izlaz, automatskog i ručnog skaliranje iz mogućnosti dostupne su projekta servisa oblak tako povećati ili smanjiti broj instanci ili virtualnih računala koje koriste program. Osim toga, možete koristiti lokalne Visual Studio za implementaciju aplikacije da biste projekta oblaka servisa Azure.

Ukratko, ako ne želite da vlasnik proširenom sloju administrativnih zadataka za prezentaciju/tvrtke i aplikacije potreban bilo koju complex konfiguracije softvera ili operacijski sustav, koristite Azure servise u Oblaku. Ako baza podataka SQL Azure ne podržava sve značajke koju tražite, koristite SQL Server u programa Azure virtualnog računala za sloju podataka. Pokretanje aplikacije na servise u Oblaku Azure i spremanje podataka u Azure virtualnim strojevima kombiniraju prednosti oba servisa. Detaljna Usporedba, potražite u odjeljku u ovoj temi na [Comparing razvoja strategije u Azure](#comparing-web-development-strategies-in-azure).

U ovaj uzorak aplikacije sloju prezentacija sadrži ulogu web koji se izvodi komponenta za servise u Oblaku u okruženju Azure izvođenja te je prilagoditi programirati web aplikacije kao što je podržano u IIS i ASP.NET. Sloju tvrtke ili pozadinskog obuhvaća tempiranja ulogu, koji se izvodi komponenta za servise u Oblaku u okruženju Azure izvođenja i korisne su za generalizirano razvoj, a mogu se izvoditi pozadine obrade web uloge. Razina baze podataka nalazi se u SQL Server virtualnog računala u Azure. Komunikacije između sloju prezentacije i razina baze podataka se ne događa izravno ili putem tvrtke sloju – komponente ulogu suradnika.

Ovaj uzorak aplikacija je korisno kada:

- Koju želite premjestiti aplikacijama virtualiziranom platforme lokalnim Azure implementacijom mogućnosti za oporavak Izrada i SQL Server visoke dostupnosti.

- Želite li vlasnik okruženju infrastrukture koje se mogu mijenjati veličinu prema gore ili dolje na zahtjev.

- Azure SQL baze podataka ne podržava sve značajke koje treba aplikacija ili u bazi podataka.

- Koju želite izvesti opterećenjem testiranje za različite razine radno opterećenje, ali u isto vrijeme ne želite vlasnik i održavanje mnogo fizičke strojeva cijelo vrijeme.

Na sljedećem su dijagramu pokazuje scenarij u lokalni i njegov oblaka omogućena rješenja. U ovom scenariju, postavite sloju prezentacija uloge web razina tvrtke uloge suradnika, ali sloju podataka na virtualnim računalima sustava u Azure. Pokretanje većeg broja primjeraka sloju prezentacije u drugoj web ulogama osigurava da biste učitali zahtjeva preko njih. Prilikom kombiniranja servise u Oblaku Azure s Azure virtualnim računalima, preporučujemo da ste postavili [Azure virtualne mreže](../virtual-network/virtual-networks-overview.md) i. Sa [Azure virtualne mreže](../virtual-network/virtual-networks-overview.md)imate stabilan i trajni privatne IP adrese unutar iste servis u oblaku u oblaku. Nakon što odredite virtualne mreže na virtualnim računalima i servise u oblaku, mogu se pokrenuti komunikaciju među same putem privatne IP adrese. Uz to, imate virtualnim strojevima i Azure web/tempiranja uloge u istoj [Azure virtualne mreže](../virtual-network/virtual-networks-overview.md) nudi niske latencije i sigurniju povezivanje. Dodatne informacije potražite u članku [što je servis u oblaku](../cloud-services/cloud-services-choose-me.md).

Kao što se vidi u dijagramu, Azure opterećenja distribuira promet preko više virtualnim strojevima i određuje koje web ili poslužitelju aplikacije za povezivanje s. Imate više instanci poslužitelja web- a aplikacije iza raspoređivača opterećenja osigurava visoke dostupnosti sloju prezentacije i sloju tvrtke. Dodatne informacije potražite u članku [najbolje prakse za aplikaciju uzorke koje je obavezna SQL HADR](#best-practices-for-application-patterns-requiring-sql-hadr).

![Uzorci aplikacije s servisima u Oblaku](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Drugi način možete implementirati ovaj uzorak aplikacije jest korištenje Konsolidirani web uloga koja sadrži prezentaciju sloju i poslovne sloju komponente kao što je prikazano na sljedećem su dijagramu. Ovaj uzorak aplikacija je korisno za programe koji zahtijevaju s praćenjem stanja dizajna. Budući da Azure nudi bez praćenja stanja računalnim čvorove na web- a tempiranja uloge, preporučujemo da implementirate logike za pohranu stanje sesije pomoću jednog od sljedećih tehnologije: [Azure predmemoriranja](https://azure.microsoft.com/documentation/services/redis-cache/), [Azure spremište tablica](../storage/storage-dotnet-how-to-use-tables.md) ili [Baze podataka SQL Azure](../sql-database/sql-database-technical-overview.md).

![Uzorci aplikacije s servisima u Oblaku](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Uzorak s Azure VMs, baze podataka Azure SQL i aplikacije servisa za Azure (Web Apps)

Primarnog cilja ovaj uzorak aplikacija je da bi se prikazala kako kombinirati Azure infrastrukture kao komponente za servis (IaaS) s Azure platforme-kao-na-servisa komponente (PaaS) u rješenje. Ovaj uzorak namijenjen je baze podataka SQL Azure za pohranu relacijskih podataka. Ne uključuje SQL Server u Azure virtualnog računala koja je dio Azure infrastrukture kao servis nudi.

U ovaj uzorak aplikacije implementacija aplikacije za baze podataka za Azure stavljanje razine prezentacije i business u istom virtualnog računala i pristupa bazi podataka na poslužiteljima SQL Azure baze podataka (SQL baza podataka). Sloju prezentaciju možete provesti pomoću klasične IIS web-mjesto utemeljeno rješenja. Ili možete implementirati objedinjenu prezentaciju i sloju tvrtke pomoću [Azure web-aplikacije](https://azure.microsoft.com/documentation/services/app-service/web/).

Ovaj uzorak aplikacija je korisno kada:

- Već imate postojeće baze podataka SQL server konfiguriran u Azure, a želite brzo testirati aplikacije.

- Koje želite testirati mogućnosti Azure okruženju.

- Želite li brzo Dodjela resursa za razvoj i testiranje okruženja za kratki vremenskog razdoblja.

- Vaše tvrtke logiku i podatke komponente access može biti samostalnih u web-aplikaciji.

Na sljedećem su dijagramu pokazuje scenarij u lokalni i njegov oblaka omogućena rješenja. U ovom scenariju, postavite razine aplikacije u jednom virtualnog računala u Azure i pristup podacima u bazi podataka SQL Azure.

![Uzorak koriste različite verzije aplikacije](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Ako se odlučite za implementaciju kombinirane web i razina aplikacije pomoću Azure web-aplikacije, preporučujemo da ostavite sloju srednjem sloju ili aplikacije kao biblioteka dinamičkih veza (DLL) u kontekstu web-aplikacije.

Osim toga, pregledajte preporuke navedene u odjeljku [Comparing web razvoja strategije u Azure](#comparing-web-development-strategies-in-azure) pri kraju ovog članka dodatne informacije o programskom tehnike.

## <a name="n-tier-hybrid-application-pattern"></a>Uzorak aplikacije hibridnog N sloja

U n sloja hibridnu aplikaciju uzorak implementirati aplikacije u više razine distributed između lokalnog i Azure. Dakle, stvorite fleksibilne i ponovno korištenje hibridnog sustava koje biste mogli izmijeniti ili dodajte određene sloju bez promjene druge razine. Da biste proširili s korporacijskom mrežom u oblak pomoću servisa [Azure virtualne mreže](../virtual-network/virtual-networks-overview.md) .

Ovaj uzorak aplikacije hibridnog je korisno kada:

- Želite da biste sastavili aplikacije koje pokreću se djelomično u oblak i djelomično lokalnog.

- Želite migrirati nekih ili svih elemenata postojeće lokalnog aplikacije u oblak.

- Koju želite premjestiti aplikacijama iz lokalnog virtualiziranom platforme Azure.

- Želite li vlasnik okruženju infrastrukture koje se mogu mijenjati veličinu prema gore ili dolje na zahtjev.

- Želite li brzo Dodjela resursa za razvoj i testiranje okruženja za kratki vremenskog razdoblja.

- Želite da se troškova učinkovit način da biste preuzeli sigurnosne kopije za enterprise aplikacija za baze podataka.

Sljedeći dijagram prikazuje uzorak aplikacije za hibridno n sloja koji obuhvaća web-mjesto lokalnom i u okvir za Azure. Kao što je prikazano u dijagramu, infrastruktura za lokalni sadrži kontroler domene [Servisa Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) da podržava provjeru autentičnosti korisnika i autorizacije. Imajte na umu da dijagram pokazuje scenarij, gdje live neki dijelovi sloju podataka u centru za podatke na lokalni dok neki dijelovi sloju podataka uživo u Azure. Ovisno o potrebama vaše aplikacije, možete provesti nekoliko drugih scenarija hibridnog. Ako, na primjer, možda Imajte sloju prezentacije i sloju tvrtke na lokalnog okruženja, no sloju podataka u Azure.

![Uzorak aplikacije N sloja](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

U Azure Active Directory kao samostalni direktorij oblaka možete koristiti za tvrtku ili ustanovu ili postojećeg lokalnog servisa Active Directory možete integrirati i s [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Kao što se vidi u dijagramu, poslovne komponente sloju možete pristupiti na više izvora podataka, kao što su za [SQL Server na Azure](virtual-machines-windows-sql-server-iaas-overview.md) putem u privatnu interne IP adrese, lokalnog sustava SQL Server putem [Virtualne mreže Azure](../virtual-network/virtual-networks-overview.md)ili s [Bazom podataka SQL](../sql-database/sql-database-technical-overview.md) pomoću tehnologije davatelj podataka za .NET Framework. U ovaj dijagram baze podataka SQL Azure je servis za pohranu neobavezne podatke.

U uzorak aplikacije hibridnog n sloja, možete implementirati sljedeće tijek rada u redoslijedu koji je naveden:

1. Odredite aplikacija za baze podataka tvrtke koje moraju se pomaknuti prema gore na cloud koristeći [Microsoft procjenu i komplet alata za planiranje (KARTA)](http://microsoft.com/map). Komplet alata za KARTE prikuplja zaliha i performanse podataka s računala se preporučuje za virtualizacije i daje preporuke na kapaciteta i procjenu planiranja.

1. Planiranje resursa i potrebna u platforme Azure, kao što su računi za pohranu i virtualnim strojevima konfiguracija.

1. Postavljanje mrežne veze između korporacijskom mrežom lokalnog i [Azure virtualne mreže](../virtual-network/virtual-networks-overview.md). Da biste postavili vezu između u korporacijskom mrežom lokalnog i virtualnog računala u Azure, koristite jedan od sljedeća dva načina:

    1. U Azure uspostaviti vezu između lokalnog i Azure putem javne krajnje točke na virtualnog računala. Ovaj postupak omogućuje jednostavno postavljanje i omogućuje korištenje provjere autentičnosti sustava SQL Server u virtualnog računala. Osim toga, postavite pravila za mrežni sigurnosne grupe da biste odredili javno promet usmjerili na VM. Dodatne informacije potražite u članku [Dopusti vanjski pristup vašem VM pomoću portala za Azure](virtual-machines-windows-nsg-quickstart-portal.md).

    1. Uspostaviti vezu između lokalnog i Azure putem tunelom Azure virtualne privatne mreže (VPN-a). Ovaj postupak omogućuje da biste proširili pravila domene virtualnog računala u Azure. Osim toga, možete postaviti pravila vatrozida i koristiti provjeru autentičnosti sustava Windows u virtualnog računala. Trenutno Azure podržava web-mjesto sigurne VPN-a web-mjesto i u okvir za točku web VPN veze:

        - S sigurnu vezu web-mjesto, možete uspostaviti mrežne veze između lokalne mreže i virtualne mreže u Azure. Preporučuje se za povezivanje lokalnog okruženja za podataka centar za Azure.

        - S sigurnu vezu točke web možete uspostaviti veza s mrežom između mrežom virtualne u Azure i na pojedinačna računala s programom bilo kojeg mjesta. Uglavnom se preporučuje za razvoj i testiranje svrhe.

    Informacije o povezivanju sa sustavom SQL Server u Azure potražite u članku [povezivanje za SQL Server virtualni stroj na Azure](virtual-machines-windows-classic-sql-connect.md).

1. Postaviti Zakazani zadaci i upozorenja koja sigurnosnu kopiju lokalne podatke u virtualnog računala disk u Azure. Dodatne informacije potražite u članku [SQL Server sigurnosna kopija i vraćanje sa servisom za spremište blobova platforme Azure](https://msdn.microsoft.com/library/jj919148.aspx) i [sigurnosno kopiranje i vraćanje za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-backup-recovery.md).

1. Ovisno o potrebama vaše aplikacije možete implementirati jedan od sljedeća tri uobičajeni scenariji:

    1. Na web-poslužitelj, poslužitelj aplikacije i razlikuje naglaske podataka možete zadržati na poslužitelju baze podataka u Azure dok držite na povjerljive podatke na lokalni.

    1. Možete zadržati web-poslužitelj i aplikacije informacije o lokalnom poslužitelju dok poslužitelj baze podataka u virtualnog računala u Azure.

    1. Možete zadržati poslužitelj baze podataka, web-poslužitelj, a aplikacija informacije o lokalnom poslužitelju dok držite replike baze podataka na virtualnim računalima sustava u Azure. Ta postavka omogućuje lokalne poslužitelje web ili aplikacijama za izvješćivanje da biste pristupili replike baze podataka u Azure. Zbog toga možete postići donji radno opterećenje u bazi podataka programa lokalnog. Preporučujemo da biste implementirati scenarij za Tamni čitati radnih opterećenja i Razvojna namjene. Informacije o stvaranju replike baze podataka u Azure potražite u članku grupe dostupnosti AlwaysOn [visoke dostupnosti](virtual-machines-windows-sql-high-availability-dr.md)i oporavak Izrada za SQL Server na virtualnim strojevima sa sustavom Azure.

## <a name="comparing-web-development-strategies-in-azure"></a>Usporedba web razvoja strategije servisu Azure

Da biste implementirati i implementacija više razina aplikacije utemeljene na SQL Server u Azure, možete koristiti jedan od sljedeća dva načina programiranje:

- Postavljanje klasične web-poslužitelj (IIS - poslužitelja Internet Information Services) u Azure i pristup baza podataka sustava SQL Server na virtualnim strojevima sa sustavom Azure.

- Implementirati i implementirati servis u oblaku za Azure. Zatim, provjerite je li servis u oblaku možete pristupiti baza podataka u sustavu SQL Server u Azure virtualnog računala. Servis u oblaku mogu sadržavati više web- a tempiranja uloge.

Sljedeća tablica nudi usporedbu klasične web-razvoju s servise u Oblaku Azure i web-aplikacije Azure vezana uz SQL Server na virtualnim strojevima sa sustavom Azure. Tablica sadrži Azure web-aplikacijama kao što je moguće koristiti SQL Server u Azure VM kao izvor podataka za Azure web-aplikacije putem njegova javno virtualna IP adresa ili naziv DNS-a.

||Tradicionalni web-razvoju virtualnim računalima sustava u Azure|Servisi u oblaku u Azure|Web-mjesta u Azure Web Apps|
|---|---|---|---|
|**Prijenos aplikacije iz lokalnog**|Postojeće aplikacije kao-je.|Aplikacije moraju web i tempiranja uloge.|Postojeće aplikacije kao-je ali prikladniji za samostalnih web-aplikacije i web-usluge koje je potrebno brzi skalabilnost.|
|**Razvoj i implementaciju**|Visual Studio, WebMatrix, Visual Web Developer, WebDeploy, FTP, TFS, Upravitelj IIS, PowerShell.|Visual Studio Azure SDK TFS, PowerShell. Svaki servis u oblaku sadrži dva okruženja na koji možete implementirati paketa i konfiguraciji: pripremna i proizvodnje. Pripremna okruženje za testiranje prije Promakni u radnog možete implementirati servis u oblaku.|Visual Studio, WebMatrix, Visual Web Developer, FTP, BROJKA, BitBucket, CodePlex, DropBox, GitHub, Mercurial, TFS, Web implementirati, PowerShell.|
|**Administracija i postavljanje**|Vi ste odgovorni za administrativne zadatke na aplikaciju, podaci, pravila vatrozida, virtualne mreže i operacijski sustav.|Vi ste odgovorni za administrativne zadatke na aplikaciju, podaci, pravila vatrozida i virtualne mreže.|Vi ste odgovorni za administrativne zadatke na aplikaciju i samo podatke.|
|**Visoke dostupnosti i oporavak Izrada (HADR)**|Preporučujemo da postavite virtualnim strojevima u istom postavljanje dostupnosti i u istom servis u oblaku. Zadržavanje vaše VMs u istom dostupnosti skupa omogućuje Azure smjestiti čvorove visoke dostupnosti u zasebnom kvara domene i nadogradnje domene. Isto tako, čuvanja vaše VMs u istom servis u oblaku omogućuje opterećenja i VMs možete izravno međusobno komuniciraju putem lokalne mreže centru za Azure podataka.<br/><br/>Vi ste odgovorni za implementaciju visoke dostupnosti i Izrada oporavak rješenja za SQL Server na virtualnim računalima sustava Azure da biste izbjegli sve isključiti. Podržani tehnologija HADR potražite u članku [visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>Vi ste odgovorni za sigurnosno kopiranje vlastite podatke i aplikacije.<br/><br/>Azure možete premjestiti virtualnim strojevima glavno računalo u centru za podataka ne uspije zbog problema s hardvera. Osim toga, tomu može biti planiranog nedostupnost vaše VM prilikom ažuriranja glavno računalo za sigurnost ili softver ažuriranja. Stoga preporučujemo da održavate najmanje dva VMs u svakom razina aplikacije da biste bili sigurni neprekinuti dostupnost. Azure ne nudi SLA za jednu virtualnog računala. Dodatne informacije potražite u članku [Azure otpornost Tehnički vodič](../resiliency/resiliency-technical-guidance.md).|Azure upravlja neuspjeha dobivenima podlozi softver hardver ili operacijski sustav. Preporučujemo da biste implementirati više instanci weba ili tempiranja uloge da biste bili sigurni visoke dostupnosti aplikacija. Informacije potražite u članku [servisi u Oblaku, virtualnim strojevima i virtualne mreže razinu ugovor o usluzi](http://www.microsoft.com/download/details.aspx?id=38427) i [oporavak Izrada i visoke dostupnosti za Azure aplikacije](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>Vi ste odgovorni za sigurnosno kopiranje vlastite podatke i aplikacije.<br/><br/>Baza podataka u bazi podataka sustava SQL Server na VM Azure residing, vi ste odgovorni za implementaciju visoke dostupnosti i Izrada oporavak rješenje da biste izbjegli sve nedostupnost. Podržani tehnologija HDAR potražite u članku visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim strojevima sa sustavom Azure.<br/><br/>**Zrcaljenje baze podataka SQL Server**: korištenje sa servisa Azure oblaka Services (uloga web-tempiranja). SQL Server VMs servisa projekta oblaka mogu i u istom Azure virtualne mreže. Ako SQL Server VM ne nalazi u istom virtualne mreže, morate je stvaranje pseudonima SQL Server da biste usmjerili komunikacije na instancu sustava SQL Server. Pseudonim mora odgovarati i naziv za SQL Server.|Visoke dostupnosti nasljeđuju od uloge Azure tempiranja, blobova platforme Azure i baze podataka SQL Azure. Ako, na primjer, Azure prostora za pohranu održava tri replike blob, tablice i red podataka. U bilo kojem trenutku baze podataka SQL Azure zadržava tri replike podataka koji se izvodi – jedan primarni replike i dvije sekundarnog replike. Dodatne informacije potražite u članku Azure i [Prostor za pohranu](https://azure.microsoft.com/documentation/services/storage/) [Baze podataka SQL Azure](../sql-database/sql-database-technical-overview.md).<br/><br/>Imajte na umu da Azure web-aplikacije ne podržava Azure virtualne mreže kada koristite SQL Server u Azure VM kao izvor podataka Azure web-aplikacijama. Drugim riječima, sve veze s web-aplikacijama Azure SQL Server VMs u Azure mora proći kroz javno krajnje točke virtualnih računala. To može uzrokovati određena ograničenja za visoke dostupnosti i Izrada scenariji za oporavak. Na primjer, klijentska aplikacija na Azure web-aplikacije povezivanje s baze podataka SQL Server VM ne bi možete povezati s novi primarni poslužitelj baze podataka potrebno postavljanje Azure virtualne mreže između VMs glavno računalo za SQL Server u Azure. Zbog toga pomoću **Zrcaljenje baze podataka SQL Server** Azure web-aplikacije nije podržan trenutno.<br/><br/>**SQL Server AlwaysOn dostupnost grupe**: možete postaviti grupe dostupnosti AlwaysOn kada Azure web-aplikacije pomoću SQL Server VMs u Azure. No morate konfigurirati AlwaysOn dostupnost grupe ga Slušatelj usmjeravanje komunikacije u primarni replike putem javne krajnje točke uravnoteženja.|
|**Povezivanje s više lokaciji**|Azure virtualne mreže možete koristiti za povezivanje s lokalnim.|Azure virtualne mreže možete koristiti za povezivanje s lokalnim.|Podržana je Azure virtualne mreže. Dodatne informacije potražite u članku [Web aplikacije virtualne Integracija s mrežom](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/).|
|**Skalabilnost**|Promjena veličine gore dostupna povećanje veličine virtualnog računala ili dodavanjem više diskova. Dodatne informacije o veličinama virtualnog računala potražite u članku [Veličine virtualnog računala za Azure](virtual-machines-windows-sizes.md).<br/><br/>**Za poslužitelj baze podataka**: skaliranje izlaz je dostupan putem tehnike za stvaranje particija baze podataka i SQL Server AlwaysOn dostupnost grupe.<br/><br/>Za velike čitanje radnih opterećenja, možete koristiti [Grupe dostupnosti AlwaysOn](https://msdn.microsoft.com/library/hh510230.aspx) na više sekundarne čvorove kao i SQL Server replikacije.<br/><br/>Za velike pisanje radnih opterećenja, mogli implementirati vodoravnog stvaranje particija podataka na više fizičke poslužiteljima omogućuju skaliranje iz aplikacije.<br/><br/>Osim toga, možete implementirati skaliranje odgovaranjem korištenju sustava [SQL Server s podacima o njima ovisne usmjeravanja](https://technet.microsoft.com/library/cc966448.aspx). S podacima o njima ovisne usmjeravanje (DDR), morate provesti mehanizam za stvaranje particija u klijentskoj aplikaciji, obično u sloju sloju tvrtke da biste usmjerili zahtjeva za baze podataka na više čvorove SQL Server. Razina tvrtke sadrži mapiranja kako je particije podatke i koje čvor sadrži željene podatke.<br/><br/>Programi koji se izvode virtualnih računala mogu se mijenjati veličinu. Dodatne informacije potražite u članku [upute za promjenu veličine aplikacije](../cloud-services/cloud-services-how-to-scale.md).<br/><br/>**Napomena o važnosti**: značajka **Automatsko skaliranje** u Azure omogućuje automatski povećali ili smanjili virtualnih računala koje koriste program. Ta značajka jamčiti doživljaja krajnjeg korisnika ne utječe na negativno tijekom razdoblja Vršna i VMs su isključena kada nedostaje na zahtjev. Preporučuje se da ne postavite mogućnost automatsko skaliranje za servis u oblaku obuhvaća SQL Server VMs. Razlog je značajka Automatsko skaliranje omogućuje je Azure da biste uključili virtualnog računala kada je veće od neke prag procesora u tom VM i kada procesora koristi dolazi manja od ga isključite virtualnog računala. Automatsko skaliranje značajka je korisna za bez praćenja stanja aplikacija, kao što je web-poslužiteljima, gdje bilo koji VM možete upravljati radno opterećenje bez reference u bilo kojem prethodno stanje. Međutim, značajka Automatsko skaliranje nije korisne su za s praćenjem stanja aplikacija, kao što je SQL Server, gdje samo jedna instanca omogućuje pisanje u bazu podataka.|Skaliranje gore dostupna pomoću više web- a tempiranja uloge. Dodatne informacije o veličinama virtualnog računala web uloge i uloge suradnika, potražite u članku [Konfiguriranje veličine za servise u Oblaku](../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Prilikom korištenja **Servisa u Oblaku**, možete definirati više uloge izlaganja obrada i i postigli fleksibilne skaliranje aplikacije. Svaki servis u oblaku sadrži jednu ili više uloge web i/ili tempiranja uloge, svaka s vlastitom datoteka aplikacije i konfiguracije. Možete skaliranje kopirane servis u oblaku povećavanjem broj instanci uloga (virtualnim strojevima) uvesti za uloge i skaliranje na padajućem servis u oblaku tako da smanjite broj instanci ulogu. Detaljne informacije potražite u članku [Azure izvođenja modela](../cloud-services/cloud-services-choose-me.md).<br/><br/>Skaliranje iz je dostupan putem ugrađene Azure visoke dostupnosti podrške putem [servisa u Oblaku, virtualnim strojevima i virtualne mreže razinu ugovor o usluzi](http://www.microsoft.com/download/details.aspx?id=38427) i opterećenja.<br/><br/>Za aplikaciju s više razina, preporučujemo da li se povezati s web-tempiranja uloge aplikacije VMs putem virtualne mreže Azure poslužitelj baze podataka. Osim toga, Azure nudi ujednačavanja za VMs u istom servisa u oblaku, opterećenja šire zahtjevima korisnika preko njih. Virtualnim strojevima povezani na taj način mogu komunicirati izravno međusobno putem lokalne mreže centru za Azure podataka.<br/><br/>Možete postaviti **Automatsko skaliranje** na portalu Azure klasični, kao i vremena za raspored. Dodatne informacije potražite u članku [upute za promjenu veličine aplikacije](../cloud-services/cloud-services-how-to-scale.md).|**Promjena veličine gore i dolje**: koje možete povećati/smanjenje veličine instance (VM) rezervirana za web-mjesta.<br/><br/>Skaliranje odgovor: možete dodati više rezervirane instanci (VMs) za web-mjesta.<br/><br/>Možete postaviti **Automatsko skaliranje** na portalu, kao i vremena za raspored. Dodatne informacije potražite u članku [na web-aplikacije mjerilo](../app-service-web/web-sites-scale.md).|

Dodatne informacije o odabiru između tih programiranje metoda potražite u članku [Azure web-aplikacije, servise u Oblaku i VMs: kada koristiti koju](../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o pokretanju sustava SQL Server u Azure virtualnog računala potražite u članku [SQL Server na pregled virtualnim strojevima Azure](virtual-machines-windows-sql-server-iaas-overview.md).
