<properties
    pageTitle="Konfiguriranje uvijek na grupe dostupnosti u Azure VM automatski - Voditelj resursa"
    description="Stvaranje uvijek na dostupnost grupe sustava s Azure virtualnim strojevima u načinu rada za Azure Voditelj resursa. Pomoću ovog praktičnog vodiča prvenstveno koristi korisničkog sučelja da biste automatski stvorili cijelu rješenja."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-always-on-availability-group-in-azure-vm-automatically---resource-manager"></a>Konfiguriranje uvijek na grupe dostupnosti u Azure VM automatski - Voditelj resursa

> [AZURE.SELECTOR]
- [Voditelj resursa: predloška](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Voditelj resursa: ručno](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klasični: korisničko Sučelje](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klasični: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

Pomoću ovog praktičnog vodiča završetka do kraja objašnjava da biste stvorili grupu dostupnost SQL Server s virtualnim strojevima Azure Voditelj resursa. Vodič koristi Azure blades Konfiguriranje predloška. Će pregled zadanih postavki, unesite potrebne postavke i ažurirati blades na portalu kao voditi kroz ovog praktičnog vodiča.

Na kraju vodič, SQL Server dostupnost grupe rješenja u Azure će se sastojati od sljedećih elemenata:

- Virtualne mreže koja sadrži više podmreže, uključujući korisničko sučelje i pozadinske podmreže

- Kontroler dvije domene domenu Active Directory (AD)

- Dva SQL Server VMs implementiran na podmreži pozadinsku i pridruženo domeni AD

- 3 čvor WSFC klaster s modelom kvorum Većina čvor

- Dostupnost grupe sustava s dvije replike sinkrono potvrdi dostupnosti baze podataka

Na slici u nastavku je grafički prikaz rješenja.

![Testiranje Laboratorija arhitektura za AG servisu Azure](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Svi resursi ovog rješenja pripadaju grupi jedan resurs.

Pomoću ovog praktičnog vodiča podrazumijeva sljedeće:

- Već imate račun za Azure. Ako nemate onaj koji se [registrirati za probnu računa](http://azure.microsoft.com/pricing/free-trial/).

- Dodjela resursa u SQL Server VM iz galerije virtualnog računala pomoću u GUI znate već. Dodatne informacije potražite u članku [Dodjeljivanje virtualnog računala za SQL Server na Azure](virtual-machines-windows-portal-sql-server-provision.md)

- Već imate pune razumijevanja dostupnost grupe. Dodatne informacije potražite u članku [uvijek na grupe dostupnosti (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Ako vas zanima korištenja grupe dostupnosti sa sustavom SharePoint, također potražite u članku [Konfiguriranje SQL Server 2012 uvijek na dostupnost grupe sustava SharePoint 2013](http://technet.microsoft.com/library/jj715261.aspx).

U ovom ćete praktičnom vodiču će koristiti Azure portala:

- Odabir u predlošku uvijek na s portala

- Pregled postavki predloška i ažuriranje nekoliko postavki konfiguriranje okruženju sustava

- Praćenje Azure tijekom stvaranja cijelo okruženje

- Povezivanje na neki od kontrolera domena, a zatim neku od SQL Server

[AZURE.INCLUDE [availability-group-template](../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]


## <a name="provision-the-cluster-from-the-gallery"></a>Dodjela resursa za klaster iz galerije

Azure nudi Galerija brzih za cijelu rješenja. Da biste pronašli predložak:

1.  Prijavite se na portal Azure pomoću računa.
1.  Na alatnoj traci Azure portala kliknite **+ Novo.** Na portalu otvorit će se novi plohu.
1.  Na novu plohu traži **AlwaysOn**.
![Traženje predloška AlwaysOn](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
1.  U rezultatima pretraživanja pronađite **Klaster AlwaysOn za SQL Server**.
![Predložak AlwaysOn](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
1.  Na **Odaberite model implementacije** odaberite **Upravitelj resursa**.

### <a name="basics"></a>Osnove

Kliknite na **Osnove** i konfigurirajte sljedeće:

- **Korisničko ime administratora** je korisnički račun administratorske dozvole za domenu i član uloge za fiksne poslužitelja sustava SQL Server na oba instance sustava SQL Server. Koristite **Postavke DomainAdmin**za ovog praktičnog vodiča.

- **Je lozinka za administratorski račun domene.** Koristite complex lozinku. Potvrdite lozinku.

- **Pretplata** je pretplatu u koju će Azure fakturu da biste pokrenuli svi resursi implementiran dostupnost grupe. Ako vaš račun ima više pretplata možete odrediti neku drugu pretplatu.

- **Grupa resursa** je naziv za će pripadati grupi sve Azure resurse stvorio ovog praktičnog vodiča. Pomoću ovog praktičnog vodiča korištenja **SQL-SLIKA-ru**. Dodatne informacije potražite u članku (pregled upravitelja resursa za Azure) [resursa-grupe – overview.md / #-grupe resursa].

- **Mjesto** je Azure područje u kojem će se stvoriti resursi za ovog praktičnog vodiča. Odaberite Azure regiju za hostiranje Infrastruktura.

U nastavku je plohu **Osnove** izgleda:

![Osnove](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

- Kliknite **u redu**.

### <a name="domain-and-network-settings"></a>Postavke domene i mreže

Ovaj predložak Azure Galerija stvara novu domenu s novi kontrolera domena. Također stvara novu mrežu i dva podmreže. Predložak omogućuju stvaranje poslužitelji u postojeću domenu ili virtualne mreže. Sljedeći je korak da biste konfigurirali postavke domene i mreže.

Na **domeni i mrežne postavke** plohu pregledajte unaprijed vrijednosti za domenu i mrežne postavke:

- **Naziv domene korijenski skupa stabala** je naziv domene koji će se koristiti za AD domenu koju će hostira klaster. Da biste postigli vodič koristite **contoso.com**.

- Virtualne mreže je **naziv** mreže za Azure virtualne mreže. Koristite **autohaVNET**za ovog praktičnog vodiča.

- Kontroler domene podmreže je **naziv** dijela virtualne mreže koji hostira kontrolerom domene. Pomoću ovog praktičnog vodiča korištenja **podmreže 1**. U ovom podmreže će koristiti adresu prefiksa **10.0.0.0/24**.

- SQL Server podmreže je **naziv** dijela virtualne mreže domaćini SQL Server i datoteke dijeliti zrcaljenja. Pomoću ovog praktičnog vodiča korištenja **podmreže 2**. U ovom podmreže će koristiti adresu prefiksa **10.0.1.0/26**.

Dodatne informacije o virtualne mreže u [Azure potražite u članku pregled virtualne mreže](../virtual-network/virtual-networks-overview.md).  

**Domene i mrežne postavke** trebao bi izgledati ovako:

![Postavke domene i mreže](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Ako je potrebno, možete promijeniti te vrijednosti. Za ovaj vodič koristimo unaprijed određene vrijednosti.

- Pregledajte postavke, a zatim kliknite **u redu**.

###<a name="availability-group-settings"></a>Postavke grupe dostupnosti

Na **Postavke grupe dostupnosti** pregledajte unaprijed određene vrijednosti za grupe dostupnosti i ga slušatelj.

- **Naziv grupe availablity** je naziv grupirani resursa za grupe dostupnosti. Pomoću ovog praktičnog vodiča korištenja **Contoso ag**.

- **dostupnost ga slušatelj naziv** koristi klaster i interne opterećenja. Klijenti za povezivanje sa sustavom SQL Server možete koristiti ovaj naziv za povezivanje s odgovarajućim replike baze podataka. Pomoću ovog praktičnog vodiča korištenja **Contoso-ga slušatelj**.

-  **dostupnost grupe ga slušatelj priključak** određuje će koristiti TCP priključak ga slušatelj SQL Server. Da biste postigli ovog praktičnog vodiča koristite zadani je priključak, **1433**.

Ako je potrebno, možete promijeniti te vrijednosti. Da biste postigli ovog praktičnog vodiča koristite unaprijed određene vrijednosti.  

![Postavke grupe dostupnosti](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

- Kliknite **u redu**.

###<a name="vm-size-storage-settings"></a>Veličina VM, postavke za pohranu

**Veličina VM** , postavke za pohranu odaberite veličinu virtualnog računala sustava SQL Server i pregledajte ostale postavke.

- **Veličina virtualnog računala sustava SQL Server** je veličine Azure virtualnog računala i SQL Server. Odaberite odgovarajući za svoje radno opterećenje veličinu virtualnog računala. Ako izrađujete ovo okruženje za vodiča **DS2**. Za proizvodnju radnih opterećenja odaberite veličinu virtualnog računala koja podržava povećavaju. **DS4** je potrebno više radnih opterećenja proizvodnje ili veća. Predložak će sastavljanje dva virtualnim strojevima takve veličine i instalirati SQL Server na svaki od njih. Dodatne informacije potražite u članku [veličine za virtualnih računala](virtual-machines-linux-sizes.md).

>[AZURE.NOTE]Azure instalirat će Enterprise Edition sustava SQL Server. Trošak ovisi o izdanju i veličine virtualnog računala. Detaljne informacije o trenutnom troškova potražite u članku [virtualnim strojevima cijene](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

- **Domene kontroler virtualnog računala veličina** je veličina virtualnog računala za kontrolera domena. Pomoću ovog praktičnog vodiča korištenja **D2**.

- **Veličina virtualnog računala zrcaljenja zajedničko korištenje datoteka** je veličine virtualnog računala zrcaljenja zajedničko korištenje datoteka. Pomoću ovog praktičnog vodiča korištenja **A1**.

- **Račun za pohranu SQL** je naziv računa za pohranu na čuvanje podataka sustava SQL Server i diskova operacijski sustav. Koristite **alwaysonsql01**za ovog praktičnog vodiča.

- **Račun za pohranu Kontroler** je naziv računa za pohranu za kontrolera domena. Koristite **alwaysondc01**za ovog praktičnog vodiča.

- **Podatke sustava SQL Server na disku veličina** u TB je veličina disk podataka sustava SQL Server u TB. Navedite broj od 1 do 4. Ovo je veličina podatkovni disk koji će se priložiti svaki SQL Server. Pomoću ovog praktičnog vodiča korištenja **1**.

- **Optimizacija za pohranu** postavlja određene prostora za pohranu konfiguracijske postavke ovisno o vrsti radno opterećenje virtualnim računalima sustava SQL Server. Svih SQL poslužitelja u ovom scenariju koristite prostora za pohranu premium s predmemorije glavno računalo za Azure disk postaviti samo za čitanje. Uz to, možete optimizirati postavke sustava SQL Server za povećavaju odabirom jedne od ove tri postavki:

    - **Općenito radno opterećenje** postavlja nema određenu konfiguraciju postavki

    - **Obrada Transactional** postavlja 1117 i 1118 zastavicu za praćenje

    - **Skladištenju** postavlja zastavicu za praćenje 1117 i 610

Pomoću ovog praktičnog vodiča korištenja **Općenito radno opterećenje**.

![Postavke za pohranu VM veličine](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

- Pregledajte postavke, a zatim kliknite **u redu**.

####<a name="a-note-about-storage"></a>Napomena o prostora za pohranu

Dodatni optimizacije ovisi o veličini diskova podataka sustava SQL Server. Za svaki terabajta podatkovni disk Azure dodaje se dodatni 1 TB premium prostor za pohranu (SSD). Kada poslužitelj zahtijeva 2 TB ili više njih, predložak stvara grupe aplikacija za pohranu na svakom SQL Server. Prostor za pohranu skup je obrazac virtualizacije prostora za pohranu koju više diskova konfigurirani tako da bi veći kapacitet, stabilnosti i performansi.  Predložak pa stvara prostora za pohranu na resurse za pohranu i to predstavlja kao jedan podatkovni operacijski sustav. Predložak određuje disk kao disk podataka za SQL Server. Predložak pronađe na resurse za pohranu za SQL Server pomoću sljedeće postavke:

- Veličina pruge je postavka interleave za virtualne disk. Transakcijskih radnih opterećenja to je postavljen na 64 KB. Za podatke skladištenje radnih opterećenja postavka je 256 KB.

- Otpornost je jednostavno (bez otpornost).

>[AZURE.NOTE] Prostor za pohranu Azure premium je lokalno suvišnih, a zadržava tri kopije podataka unutar jedno područje tako da se ona nije potrebna dodatna otpornost na resurse za pohranu.

- Broj stupaca jednak broj diskova u prostor za pohranu.

Dodatne informacije o prostora za pohranu prostor i pohranu grupe potražite u članku:

- [Pregled prostora za pohranu razmake](http://technet.microsoft.com/library/hh831739.aspx).

- [Sigurnosno kopiranje sustava Windows Server i grupe za pohranu](http://technet.microsoft.com/library/dn390929.aspx)

Dodatne informacije o najbolje prakse za konfiguraciju sustava SQL Server potražite u članku [performanse najbolje prakse za SQL Server na virtualnim računalima za Azure](virtual-machines-windows-sql-performance.md)


###<a name="sql-server-settings"></a>Postavke sustava SQL Server

O **postavkama sustava SQL Server** pregledajte i promijenite prefiks naziva SQL Server VM, verzija programa SQL Server, račun servisa SQL Server i lozinku, a automatski SQL zakrpa održavanja raspored.

- **SQL Server naziv prefiks** se koristi za stvaranje naziva za svaki SQL Server. Pomoću ovog praktičnog vodiča korištenja **Contoso ag**. SQL Server imena bit će *Contoso ag 0* i *Contoso-ag-1*.

- **Verzija sustava SQL Server** je verzija sustava SQL Server. Pomoću ovog praktičnog vodiča korištenja **2014 SQL Server**. Možete odabrati i **SQL Server 2012** ili **SQL Server 2016**.

- **Korisničko ime za račun servisa SQL Server** je naziv domene računa za servis sustava SQL Server. Koristite **sqlservice**za ovog praktičnog vodiča.

- **Je lozinka za račun servisa SQL Server.**  Koristite complex lozinku. Potvrdite lozinku.

- **SQL automatski zakrpa raspored održavanja** označava dana u tjednu Azure automatski zakrpu SQL poslužitelja. Za ovaj vodič upišite **nedjelja**.

- **SQL automatski zakrpa sat početka održavanja** je doba dana za Azure regija kada automatsko zakrpa će početi.

>[AZURE.NOTE]Prozor kojemu za svaki VM je odaberite matricu jedan sat. Samo jedan virtualnog računala je patched istovremeno da bi se spriječilo prekidu servisa.

![Postavke sustava SQL Server](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Pregledajte postavke, a zatim kliknite **u redu**.

###<a name="summary"></a>Sažetak

Na stranici sažetak Azure Provjeri valjanost postavki. Možete preuzeti i predložak. Pregledajte sažetak. Kliknite **u redu**.

###<a name="buy"></a>Kupnja

Ovaj konačni plohu sadrži **uvjete korištenja**, a **pravilnik o zaštiti privatnosti**. Pregledajte ove podatke. Kada ste spremni za Azure da biste započeli stvarati virtualnim strojevima i sve ostale potrebne resurse za grupe dostupnosti, kliknite **Stvori**.

Portal za Azure će stvoriti grupu resursa i sve resurse.

##<a name="monitor-deployment"></a>Uvođenje monitora

Praćenje napretka implementaciju s portala za Azure. Ikona koja predstavlja implementacijskih automatski prikvačena na Azure portala nadzorne ploče.

![Azure nadzorne ploče](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

##<a name="connect-to-sql-server"></a>Povezivanje sa sustavom SQL Server

Izvodi nove instance sustava SQL Server na virtualnim strojevima koji nemaju veze s Internetom. Međutim, kontrolera domena imati internetsku nasuprotne veze. Da bi se povezuje s poslužiteljima SQL putem udaljene radne površine, prvi RDP na neki od kontrolera domena. S kontrolerom domene otvorite drugi RDP sa sustavom SQL Server.

Da biste RDP s kontrolerom primarne domene, slijedite ove korake:

1.  S Azure portala nadzorne ploče vrlo je uspio uvođenje.

1.  Kliknite **Resursi**.

1.  U plohu **resurse** kliknite **ad primarni kontroler** koji je naziv računala virtualnog računala za kontroler primarne domene.

1.  Na plohu za **ad primarni kontroler** kliknite **Poveži**. Vaš preglednik će vas pitati želite li otvoriti ili spremiti udaljene objekt. Kliknite **Otvori**.
![Povezivanje s Kontroler](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/13-ad-primary-dc-connect.png)
1.  **Udaljena radna površina** upozorit će publisher ovaj udaljene nije moguće prepoznati. Kliknite **Poveži**.

1.  Sigurnost sustava Windows traži da unesete vjerodajnice za povezivanje se s IP adresom kontrolera primarne domene. Kliknite **neki drugi račun**. Za **korisničko ime** upišite **contoso\DomainAdmin**. Ovo je račun koji ste odabrali za korisničko ime administratora. Koristite complex lozinku koji ste odabrali kada ste konfigurirali predložak.

1.  **Udaljena radna površina** upozorit će da udaljeno računalo nije moguće provjeriti autentičnost došlo do problema sa sigurnosnim certifikatom. Ga vidjet ćete naziv certifikata sigurnost. Ako ste pratili vodič naziv bit će **ad primarni dc.contoso.com**. Kliknite **da**.

Sada ste povezani s kontrolerom primarne domene. Da biste RDP sa sustavom SQL Server, slijedite ove korake:

1.  Na kontrolerom domene, otvorite **Udaljenu radnu površinu**.

1.  Za **računalo**, upišite naziv jednog od SQL poslužitelja. Za ovaj vodič upišite **sqlserver 0**.

1.  Koristite isti korisnički račun i lozinku koju ste koristili za RDP s kontrolerom domene.

Sada su povezani s RDP sa sustavom SQL Server. Otvorite SQL Server management studio, povezivanje s zadane instance sustava SQL Server i provjerite je li grupi availabilty konfiguriran.


