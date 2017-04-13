<properties
    pageTitle="Dodjela virtualnog računala za SQL Server | Microsoft Azure"
    description="Stvaranje i povezivanje s virtualnog računala sustava SQL Server u Azure pomoću portala za. Pomoću ovog praktičnog vodiča koristi MOD upravitelja resursa."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    editor=""
    manager="jhubbard"
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-in-the-azure-portal"></a>Dodjela resursa za SQL Server virtualnog računala na portalu za Azure

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShell](virtual-machines-windows-ps-sql-create.md)

Pomoću ovog praktičnog vodiča završetka do kraja pokazuje kako koristiti Azure Portal Dodjela virtualnog računala koja se izvodi SQL Server.

Galerija Azure virtualnog računala (VM) sadrži nekoliko slike koje sadrže Microsoft SQL Server. Uz samo nekoliko klikova odaberite jednu od SQL VM slike iz galerije i dodjela resursa u okruženju sustava Azure.

U ovom ćete praktičnom vodiču ćete:

- [Odaberite sliku SQL VM iz galerije](#select-a-sql-vm-image-from-the-gallery)
- [Konfiguriranje i stvoriti na VM](#configure-the-vm)
- [Otvaranje u VM s udaljene radne površine](#open-the-vm-with-remote-desktop)
- [Daljinsko povezivanje sa sustavom SQL Server](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-the-gallery"></a>Odaberite sliku SQL VM iz galerije

1. Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa.

    >[AZURE.NOTE] Ako nemate račun za Azure, posjetite [Azure slobodno probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

1. Na portalu Azure kliknite **Novo**. Na portalu otvorit će se **Nova** plohu. Resursi za SQL Server VM su u grupi **virtualnim strojevima** na tržištu.

1. U plohu **Novo** kliknite **virtualnih računala**.

1. Da biste vidjeli sve dostupne slike, kliknite **Pogledajte sve** plohu **virtualnih računala** .

    ![Azure virtualnim strojevima plohu](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade.png)

1. U odjeljku **poslužitelja baze podataka**, kliknite **SQL Server**. Možda ćete se morati pomaknuti prema dolje da biste pronašli **poslužitelja baze podataka**. Pregledajte dostupne predloške sustava SQL Server.

    ![Galerija virtualnog računala SQL slike](./media/virtual-machines-windows-portal-sql-server-provision/virtual-machine-gallery-sql-server.png)

1. Svaki predložak služi za identifikaciju verzija sustava SQL Server i operacijski sustav. Odaberite jednu od ovih slike s popisa. Pregledajte pojedinosti plohu koji sadrži opis slike virtualnog računala.

    >[AZURE.NOTE] SQL VM slike u cijene-minutni VM stvorite uključiti licenciranja troškova za SQL Server. Postoji neku drugu mogućnost Premjesti-vaše – vlasnik – licence (BYOL) i plaćanje samo za na VM. Nazive slika mjestu su s {BYOL}. Dodatne informacije o tu mogućnost, potražite u članku [Početak rada sa sustavom SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md).

1. U odjeljku **Odaberite model implementacije**, provjerite je li odabran **Voditelj resursa** . Voditelj resursa je model preporučene implementacije za nove virtualnim računalima. Kliknite **Stvori**.

    ![Stvaranje SQL VM s Voditelj resursa](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-the-vm"></a>Konfiguriranje na VM
Postoji pet blades za konfiguriranje SQL Server virtualnog računala.

| Korak               | Opis                          |
|---------------------|-------------------------------|
| **Osnove**              | [Konfiguriranje osnovne postavke](#1-configure-basic-settings)      |
| **Veličina**                | [Odabir veličine virtualnog računala](#2-choose-virtual-machine-size)   |
| **Postavke**            | [Konfiguriranje značajke](#3-configure-optional-features)   |
| **Postavke sustava SQL Server** | [Konfiguriranje postavki za SQL server](#4-configure-sql-server-settings) |
| **Sažetak**             | [Pregledajte sažetak](#5-review-the-summary)            |

## <a name="1-configure-basic-settings"></a>1. konfiguriranje osnovne postavke
Na plohu **Osnove** navedite sljedeće podatke:

* Unesite jedinstveni virtualnog računala **naziv**.
* Navedite **korisničko ime** za lokalni administratorski račun na na VM. Račun će se dodati uloga fixed poslužitelja **sustava** SQL Server.
* Navedite jaku **lozinku**.
* Ako imate više pretplata, provjerite je li pretplatu za novu VM.
* U okvir za **grupu resursa** upišite naziv za novu grupu resursa. Osim toga, da biste koristili postojeći resurs grupe kliknite **Odaberite postojeći**. Grupa resursa je zbirka povezanih resursa u Azure (virtualnim strojevima račune za pohranu, virtualne mreže, itd.).

    >[AZURE.NOTE] Korištenje novu grupu resursa korisno je ako su samo testiranje ili više informacija o implementaciji sustava SQL Server u Azure. Nakon što završite s testiranju, izbrišite grupu resursa za automatsko brisanje na VM i svi resursi pridružen toj grupi resursa. Dodatne informacije o grupama resursa potražite u članku [Pregled upravljanja resursima Azure](../azure-resource-manager/resource-group-overview.md).

* Odaberite **mjesto** u ovom implementacije.
* Kliknite **u redu** da biste spremili postavke.

    ![SQL osnove plohu](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. Odaberite veličinu virtualnog računala
U koraku **Veličina** odabir veličine virtualnog računala u plohu **Odaberite veličinu** . Na plohu početku prikazuje veličine preporučene računala koji se temelji na predlošku koji ste odabrali. Također se procjenjuje mjesečni trošak da biste pokrenuli u VM.

![Mogućnosti veličine SQL VM](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Radni radnih opterećenja, preporučujemo da odaberete veličinu virtualnog računala koji podržava [Premium prostora za pohranu](../storage/storage-premium-storage.md). Ako ne zahtijeva te razine performanse, poslužite se gumbom **Prikaži sve** , koji prikazuje sve mogućnosti veličina računala. Ako, na primjer, manja veličina računalu koji koristite za razvoj ili okruženje za testiranje.

>[AZURE.NOTE] Dodatne informacije o virtualnog računala veličine potražite u odjeljku [Veličina za virtualnih računala](virtual-machines-windows-sizes.md). Pitanja o veličinama SQL Server VM potražite u članku [performanse najbolje prakse za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-performance.md).

Odabir veličine vaše računalo, a zatim **Odaberite**.

## <a name="3-configure-optional-features"></a>3. konfigurirajte dodatne značajke
Na plohu **postavki** konfiguriranje Azure prostor za pohranu, rad na mreži i nadzor za virtualnog računala.

- U odjeljku **prostora za pohranu**, navedite na **disku upišite** standardno ili Premium (SSD). Prostor za pohranu Premium preporučuje radnih opterećenja proizvodnje.

>[AZURE.NOTE] Ako ste odabrali Premium (SSD) za veličinu stroj koji podržavaju pohranu Premium, automatski se mijenja veličina poštanskog stroj.  

- U odjeljku **račun za pohranu**, možete prihvatiti naziv računa automatski Dodjela resursa za pohranu. Možete kliknuti i na **račun za pohranu** da biste odabrali postojeći račun i konfigurirate vrstu računa za pohranu. Prema zadanim postavkama Azure stvara novi račun za pohranu s lokalno suvišnih prostora za pohranu. Dodatne informacije o mogućnostima za pohranu potražite u članku [replikacije Azure prostora za pohranu](../storage/storage-redundancy.md).

- U odjeljku **mreža**možete prihvatiti automatski popunjena vrijednosti. Možete i kliknuti na svaku značajku za ručno konfiguriranje **virtualne mreže**, **podmreže**, **javnu IP adresa**i **Mreže sigurnosne grupe**. Za potrebe ovog praktičnog vodiča, ostavite zadane vrijednosti.

- Azure omogućuje **praćenja** prema zadanim postavkama računa isti prostor za pohranu namijenjene na VM. Možete promijeniti te postavke.

- U odjeljku **Postavljanje dostupnosti**, navedite skupa dostupnost. Za potrebe ovog praktičnog vodiča, možete odabrati **ništa**. Ako namjeravate postavljanje grupe dostupnosti AlwaysOn SQL konfiguriranje dostupnosti da biste izbjegli vraćanju virtualnog računala.  Dodatne informacije potražite u članku [Upravljanje strojeva za dostupnost virtualne](virtual-machines-windows-manage-availability.md).

Kada završite konfiguriranje te postavke, kliknite **u redu**.

## <a name="4-configure-sql-server-settings"></a>4. Konfiguriranje postavki poslužitelja SQL
Na plohu **SQL Server postavke** konfigurirajte određene postavke i optimizacije za SQL Server. Postavke koje možete konfigurirati za SQL Server obuhvaćaju sljedeće.

| Postavka               |
|---------------------|
| [Povezivanje](#connectivity)              |
| [Provjera autentičnosti](#authentication)                |
| [Konfiguriranje prostora za pohranu](#storage-configuration)            |
| [Automatski zakrpa](#automated-patching) |
| [Automatske sigurnosnog kopiranja](#automated-backup)             |
| [Integracija Azure sigurnog ključa](#azure-key-vault-integration)             |
| [R Services](#r-services) |

### <a name="connectivity"></a>Povezivanje
U odjeljku **Povezivanje SQL**odredili vrstu želite instancu sustava SQL Server na ovom VM pristup. Za potrebe ovog praktičnog vodiča, odaberite **javno (internet)** da dopušta veze sa sustavom SQL Server s računala ili servisa na Internetu. Tu mogućnost, Azure automatski konfigurira vatrozid i sigurnosne grupe mreže da dopušta promet na priključak 1433.  

![Mogućnosti povezivanja za SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

Da biste se povezali sa sustavom SQL Server putem Interneta, također morate omogućiti SQL Server provjeru autentičnosti, što je opisano u sljedećem odjeljku.

>[AZURE.NOTE] Nije moguće dodati dodatna ograničenja za komunikaciju mreže vaše VM SQL Server. To možete učiniti tako da uredite sigurnosne grupe mreže nakon stvaranja na VM. Dodatne informacije potražite u članku [što je mreža grupe sigurnosti (NSG)?](../virtual-network/virtual-networks-nsg.md)

Ako želite omogućiti veze modul baze podataka putem Interneta, odaberite neku od sljedećih mogućnosti:

- **Lokalni (unutar samo VM)** da dopušta veze sa sustavom SQL Server samo iz na VM.
- **Privatni (unutar virtualne mreže)** da dopušta veze sa sustavom SQL Server s računala ili servisa u istom virtualne mreže.

>[AZURE.NOTE] Slika virtualnog računala za SQL Server Express edition Omogući automatsko TCP/IP protokola. To vrijedi čak i za mogućnosti povezivanja za javne i privatne. Za Express edition, morate koristiti Upravitelj konfiguracije SQL poslužitelja [ručno](#configure-sql-server-to-listen-on-the-tcp-protocol) omogućavanje TCP/IP protokola nakon stvaranja na VM.

Općenito govoreći, poboljšanje sigurnosti tako da odaberete povezivanje najčešće ograničenja koja omogućuje scenariju. No nisu sve mogućnosti sigurnosnog putem mreže sigurnosne grupe pravila i provjeru autentičnosti SQL/sustava Windows.

**Priključak** po zadanom je 1433. Možete navesti broj drugi priključak.
Dodatne informacije potražite u članku [povezivanje za SQL Server virtualni stroj (Voditelj resursa) | Microsoft Azure](virtual-machines-windows-sql-connect.md).

### <a name="authentication"></a>Provjera autentičnosti
Ako zahtijeva provjeru autentičnosti sustava SQL Server, u odjeljku **Provjera autentičnosti SQL**kliknite **Omogući** .

![Provjera autentičnosti sustava SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

>[AZURE.NOTE] Ako planirate pristup sustava SQL Server putem Interneta (odnosno mogućnost veze s javnim), morate omogućiti SQL ovdje provjeru autentičnosti. Pristup javnim SQL Server zahtijeva korištenje SQL provjeru autentičnosti.

Ako omogućite provjera autentičnosti sustava SQL Server, navedite **korisničko ime** i **lozinku**. Ovo korisničko ime je konfiguriran kao prijava za SQL Server provjeru autentičnosti i član **sustava** fiksno uloga poslužitelja. Dodatne informacije o načina provjere autentičnosti potražite u članku [Odabir načina provjere autentičnosti](http://msdn.microsoft.com/library/ms144284.aspx) .

Ako ne omogućite SQL Server provjeru autentičnosti, zatim koristite lokalnog administratorskog računa na na VM povezati instancu sustava SQL Server.

### <a name="storage-configuration"></a>Konfiguriranje prostora za pohranu
Kliknite **prostor za pohranu konfiguracije** da biste odredili preduvjeti za pohranu.

![Konfiguracija SQL prostora za pohranu](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

>[AZURE.NOTE] Ako ste odabrali standardni prostora za pohranu, ta mogućnost nije dostupna. Automatsko spremanje optimizaciju je dostupna samo za pohranu Premium.

Preduvjeti možete odrediti kao ulaza i izlaza operacije u sekundi (IOPs), propusnost u MB/s, a veličina ukupan prostor za pohranu. Konfiguriranje ove vrijednosti pomoću ljestvice sliding. Na portalu automatski izračunava broj diskova te potrebama.

Prema zadanim postavkama Azure optimizira prostora za pohranu za 5000 IOPs, 200 MB prostora i 1 TB prostora za pohranu. Možete promijeniti postavke za pohranu na temelju radno opterećenje. U odjeljku **optimizirana za pohranu**, odaberite jednu od sljedećih mogućnosti:

- **Općenito** je zadana postavka te podržava većina radnih opterećenja.
- Obrada **Transactional** optimizira prostora za pohranu za radnih opterećenja OLTP običnoj bazi podataka.
- Prostor za pohranu za analitički i izvješćivanja radnih opterećenja **skladištenju** optimizira.

>[AZURE.NOTE] Gornji ograničenja klizača razlikuju se ovisno o veličini odabrane virtualnog računala.

### <a name="automated-patching"></a>Automatski zakrpa
**Automatski zakrpa** omogućena je prema zadanim postavkama. Automatsko zakrpa omogućuje Azure da biste automatski zakrpu SQL Server i operacijski sustav. Navedite danu tjedna, vrijeme i trajanje za održavanje prozora. Azure izvodi zakrpa u tom prozoru održavanja. Raspored prozora Održavanje koristi regionalne postavke VM put. Ako ne želite da se Azure da biste automatski zakrpu SQL Server i operacijskom sustavu, kliknite **Onemogući**.  

![SQL automatski zakrpa](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Dodatne informacije potražite u članku [Automatski zakrpa za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automatske sigurnosnog kopiranja
Omogućivanje automatskog sigurnosne kopije baze podataka za sve baze podataka u odjeljku **Automatsko sigurnosno kopiranje**. Automatske sigurnosnog kopiranja onemogućeno je prema zadanim postavkama.

Kada omogućite SQL automatske sigurnosnog kopiranja, možete konfigurirati sljedeće:

- Razdoblje zadržavanja (broj dana) sigurnosnih kopija
- Račun za pohranu za korištenje sigurnosnih kopija
- Mogućnost šifriranja i lozinke za sigurnosne kopije

Da biste šifrirali sigurnosnog kopiranja, kliknite **Omogući**. Zatim navedite **lozinku**. Azure stvara certifikata za šifriranje sigurnosnih kopija i koristi određenu lozinku da biste zaštitili taj certifikat.

![SQL automatskog sigurnosnog kopiranja](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup.png)

 Dodatne informacije potražite u članku [Automatsko sigurnosno kopiranje za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Integracija Azure sigurnog ključ
Da biste spremili tajne sigurnost u Azure za šifriranje, kliknite **Azure ključa sigurnog integracije** , a zatim **Omogući**.

![SQL Azure ključa sigurnog Integracija](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

U sljedećoj su tablici navedeni parametre morati konfigurirao integraciju sigurnog ključ za Azure.

|PARAMETAR|OPIS|PRIMJER|
|----------|----------|-------|
|**Ključni sigurnog URL-a** |Mjesto ključa zbirke ključeva.|https://contosokeyvault.vault.Azure.NET/ |
|**Glavni naziv** |Azure Active Directory glavni naziv servisa. Ovaj naziv naziva se i kao ID klijenta.  |fde2b411 - 33d 5-4e11-af04eb07b669ccf2|
| **Glavni tajna**|Azure Active Directory servisa glavni tajna. U ovom tajna naziva se i tajna klijenta. | 9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM =|
|**Naziv vjerodajnica**|**Naziv vjerodajnica**: Integracija AKV stvara vjerodajnica unutar sustava SQL Server, dopustite VM da biste imali pristup ključa zbirke ključeva. Odaberite naziv ove vjerodajnice.| mycred1|

Dodatne informacije potražite u članku [Konfiguriranje Azure ključ sigurnog Integracija za SQL Server na Azure VMs](virtual-machines-windows-ps-sql-keyvault.md).

Kada završite s Konfiguriranje postavki za SQL Server, kliknite **u redu**.

### <a name="r-services"></a>R services
Za SQL Server 2016 Enterprise edition, imate mogućnost da biste omogućili [R usluga za SQL Server](https://msdn.microsoft.com/library/mt604845.aspx). Omogućuje korištenje naprednom analitikom sa SQL Server 2016. Kliknite **Omogući** plohu **Postavke SQL poslužitelja** .

![Omogućivanje servisa SQL Server R](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

>[AZURE.NOTE] Za SQL Server slike koje nisu 2016 Enterprise edition, mogućnost za omogućivanje servisa, R je onemogućen.

## <a name="5-review-the-summary"></a>5. Pregledajte sažetak
Na **Sažetak** plohu pregledajte sažetak, a zatim kliknite **u redu** da biste stvorili SQL Server, grupa resursa i resurse za ovaj VM.

Možete nadzirati implementaciju s portala za azure. Gumb **obavijesti** pri vrhu zaslona prikazuje se osnovni status implementacije.

>[AZURE.NOTE] Da vam pruži ideju na implementaciju puta, mogu uvesti SQL VM s područjem Istočni sad sa zadanim postavkama. Ovaj test implementacije trajalo Ukupno 26 minuta. No koje se mogu pojaviti na brže ili sporije vrijeme uvođenja ovisno o vašoj regiji, a odabrali postavke.

## <a name="open-the-vm-with-remote-desktop"></a>Otvaranje u VM s udaljene radne površine

Poduzmite sljedeće korake za povezivanje s virtualnog računala putem udaljene radne površine:

1. Kada je ugrađena Azure VM, pojavit će se ikona na VM na nadzornoj ploči za Azure. Možete pronaći i ga pregledavanjem postojeće virtualnih računala. Kliknite novi SQL virtualnog računala. **Virtualnog računala** plohu prikazuje pojedinosti virtualnog računala.
1. Pri vrhu plohu **virtualnog računala** , kliknite **Poveži**.
1. Web-pregledniku preuzimanja datoteku RDP za na VM. Otvorite datoteku RDP.
    ![Udaljena radna površina da biste SQL VM](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
1. Remote Desktop Connection obavještava publisher ovaj udaljene nije moguće prepoznati. Kliknite **Poveži** da biste nastavili.
1. U dijaloškom okviru **Sigurnost sustava Windows** , kliknite **neki drugi račun**.
1. Za vrstu **korisničko ime** ** \<korisničko ime >**, pri čemu <user name> koji ste naveli kada ste konfigurirali u VM korisničko ime. Morate dodati Početna kosa crta prije naziva.
1. Unesite **lozinku** koju ste prethodno konfigurirali za ovaj VM, a zatim **u redu** da biste se povezali.
1. Ako drugi **Udaljene radne površine** dijaloški vas pita želite li da biste se povezali, kliknite **da**.

Kada se povežete s virtualnog računala sustava SQL Server, možete pokrenuti SQL Server Management Studio i povezati se s provjeru autentičnosti sustava Windows pomoću vjerodajnica za lokalni administrator. Ako ste omogućili provjera autentičnosti sustava SQL Server, možete povezati s provjerom autentičnosti SQL pomoću prijava SQL i lozinke koje ste konfigurirali prilikom dodjele resursa.

Pristup računalu omogućuje vam da biste izravno promijenili stroj i SQL Server postavke prema svojim potrebama. Na primjer, možete konfigurirati postavke vatrozida ili promijeniti postavke konfiguracije SQL poslužitelja.

## <a name="connect-to-sql-server-remotely"></a>Daljinsko povezivanje sa sustavom SQL Server

U ovom ćete praktičnom vodiču smo odabrana **javno** pristup virtualnog računala i **Provjera autentičnosti SQL poslužitelja**. Ove postavke automatski konfigurirana virtualnog računala da dopušta veze za SQL Server s bilo kojeg klijenta putem Interneta (ako imaju odgovarajuće Prijava na SQL).

>[AZURE.NOTE] Ako niste odabrali javno prilikom dodjele resursa, dodatni koraci su potrebne za pristup instancu sustava SQL Server putem Interneta. Dodatne informacije potražite u članku [Povezivanje da biste je SQL Server virtualnog računala](virtual-machines-windows-sql-connect.md).

U sljedećim se odjeljcima pokazati kako povezati instancu sustava SQL Server na vašem VM s nekog drugog računala putem Interneta.

> [AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Daljnji koraci
Druge informacije o korištenju sustava SQL Server u Azure potražite u članku [SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md) i [Najčešća pitanja](virtual-machines-windows-sql-server-iaas-faq.md).

Video pregled sustava SQL Server na virtualnim strojevima sa sustavom Azure, pogledajte [Azure VM je najbolje platformu za SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016).

[Istraživanje tečaj](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) za SQL Server na virtualnim računalima za Azure.
