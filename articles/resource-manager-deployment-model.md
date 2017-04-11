<properties
   pageTitle="Voditelj resursa i klasični implementacije | Microsoft Azure"
   description="U članku se opisuje razlike između model implementacije Voditelj resursa i na klasičnu (ili upravljanje servisom) model implementacije."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-the-state-of-your-resources"></a>Azure Voditelj resursa nasuprot uvođenje klasičnog: razumjeti implementacije modela i stanje resursa

U ovoj se temi informacije o web-mjesto Voditelj resursa Azure i u okvir za stanje resursa, uvođenje klasičnog modele i zašto su resursa u uveden s jednim ili na drugi. Voditelj resursa i uvođenje klasičnog modeli predstavljaju dva različita načina upravljanja vašeg rješenja Azure i implementacija. Radite s njima putem dvije različite skupove API-JA, a distribuiranih resursa može sadržavati neke važne razlike. Dva modela nisu potpuno kompatibilna međusobno. U ovoj se temi opisuju te razlike.

Da biste pojednostavnili implementacije i upravljanja resursima, Microsoft preporučuje korištenje resursima za sve nove resurse. Ako je to moguće, Microsoft preporučuje ponovno implementirate postojećih resursa putem upravitelja resursa.

Ako niste upoznati s upraviteljem resursa, preporučujemo vam da najprije pregledajte terminologija definirano u [Pregled Azure Voditelj resursa](azure-resource-manager/resource-group-overview.md).

## <a name="history-of-the-deployment-models"></a>Povijest modela implementacije

Azure izvorno navedeni su samo model klasični implementacije. U ovom modelu se svaki resurs postojao neovisno; Došlo je nije moguće grupirati povezani resursi. Umjesto toga morali ručno odabir resursa koji se sastoje rješenja ili aplikaciju za praćenje i Imajte na umu da biste upravljali njima u usklađenih pristup. Da biste implementirali rješenje, morali stvaranje svaki resurs pojedinačno putem portala za klasični ili stvoriti skriptu koja implementiran u resursima ispravnim redoslijedom. Da biste izbrisali rješenje, morali ste izbrisali svaki resurs pojedinačno. Jednostavno ne može primijeniti i ažuriranje pravila za kontrolu pristupa za povezani resursi. Na kraju, koje nije moguće primijeniti oznake da biste resursa koji će ih naljepnice s uvjetima koje olakšavaju praćenje resursa i upravljanje naplatom.

U 2014., Azure uvedena Voditelj resursa koja dodaje pojam grupu resursa. Grupa resursa je spremnik za resurse koji zajednički koristite uobičajenih životni ciklus. Model implementacije resursima pruža nekoliko prednosti:

- Možete uvesti, upravljanje i nadzirati sve servise za rješenje grupe umjesto zadužen za te servise pojedinačno.
- Možete više puta implementaciju rješenja tijekom njegova životnog ciklusa, a pouzdanosti su resursa u uveden u ujednačeno stanje.
- Kontrola pristupa možete primijeniti na sve resurse u grupu resursa, a ta pravila automatski se primjenjuju prilikom dodavanja nove resurse u grupu resursa.
- Oznake možete primijeniti na resurse da biste logično organizirali sve resursa u svoju pretplatu.
- Da biste odredili infrastrukturu za rješenje možete koristiti JavaScript objekt notaciju (JSON). Datoteka JSON poznato je kao predloška Voditelj resursa.
- Možete definirati međuzavisnosti resurse tako da ih uvode ispravnim redoslijedom.

Kad je dodana je Voditelj resursa, svi resursi retroactively dodani zadane grupe resursa. Ako stvorite resursa putem klasične implementacije sada, resursa automatski se stvara u zadanu grupu resursa za tu uslugu, čak i ako niste naveli toj grupi resursa na implementaciju. Međutim, samo postojeće unutar grupe resursa ne znači da je pretvorena resursa u model upravljanja resursima. Ne možemo ćete pogledajte način rukovanja svaki servis modelima dva implementacije u sljedećem odjeljku. 

## <a name="understand-support-for-the-models"></a>Razumijevanje podrška za u modelima 

Pri odabiru model implementacije koji želite koristiti za resurse, tri su scenarija Imajte na umu:

1. Servis podržava Voditelj resursa i sadrži samo jednu vrstu.
2. Servis podržava Voditelj resursa, ali omogućuje dvije vrste: jedan za Voditelj resursa i jedan za klasični. Taj se scenarij odnosi samo na virtualnim računalima, račune za pohranu i virtualne mreže.
3. Servis ne podržava Voditelj resursa.

Da biste otkrili podržava li servis Voditelj resursa, potražite u članku [Upravljanje resursima podržane davatelje usluga](resource-manager-supported-services.md).

Ako servis koji želite koristiti ne podržava Voditelj resursa, morate nastavite koristiti klasične implementacije.

Ako servis podržava Voditelj resursa i **nije** virtualnog računala, račun za pohranu ili virtualne mreže, Voditelj resursa možete koristiti bez sve komplikacije.

Za virtualnim strojevima, račune za pohranu i virtualne mreže, ako je resurs stvoren putem klasične implementacije morate nastaviti na njoj raditi putem klasične operacije. Ako virtualnog računala, računa za pohranu ili virtualne mreže stvoren putem resursima implementaciju, morate nastavite koristiti operacija Voditelj resursa. Ovaj status možete dobiti pregledniji kada pretplate sadrži kombinacije resursa stvorene putem upravitelja resursa i klasični implementacije. Ovu kombinaciju resursa možete stvoriti neočekivane rezultate jer resurse ne podržavaju iste operacije.

U nekim slučajevima naredbu Voditelj resursa možete dohvatiti informacije o resursu putem klasične implementacije ili izvršava Administrativni zadatak kao što su premještanje klasični resursa u drugu grupu resursa. No tim slučajevima mora dati dojam podržava li vrstu operacije Voditelj resursa. Recimo da imate grupu resursa koji sadrži virtualnog računala koji je stvoren klasični implementacije. Ako pokrenite sljedeću naredbu komponente PowerShell Voditelj resursa:

    Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines

Vraća virtualnog računala:
    
    Name              : ExampleClassicVM
    ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
    ResourceName      : ExampleClassicVM
    ResourceType      : Microsoft.ClassicCompute/virtualMachines
    ResourceGroupName : ExampleGroup
    Location          : westus
    SubscriptionId    : {guid}

Međutim, resursima cmdlet **Get-AzureRmVM** vraća samo virtualnim strojevima implementiran putem upravitelja resursa. Sljedeća naredba vratite virtualnog računala putem klasične implementacije.

    Get-AzureRmVM -ResourceGroupName ExampleGroup

Samo resursa stvorene pomoću oznaka Voditelj resursa podrške. Oznake nije moguće primijeniti na klasični resurse.

## <a name="resource-manager-characteristics"></a>Voditelj resursa značajke

Da biste lakše razumijevanje dva modela, pogledajmo osobina vrsta Voditelj resursa:

- Stvara se putem [portala za Azure](https://portal.azure.com/).

     ![Portal za Azure](./media/resource-manager-deployment-model/portal.png)

     Za računalnim, za pohranu i mrežni resursi, imate mogućnost korištenja Voditelj resursa ili klasični implementacije. Odaberite **Upravitelj resursa**.

     ![Voditelj resursa implementacije](./media/resource-manager-deployment-model/select-resource-manager.png)

- Stvara s resursima verziju cmdleta Azure PowerShell. Te naredbe je u obliku *Glagolski AzureRmNoun*.

        New-AzureRmResourceGroupDeployment

- Stvara se kroz [Azure resursima REST API -JA](https://msdn.microsoft.com/library/azure/dn790568.aspx) za OSTALE operacije.

- Stvoriti pomoću naredbe EŽA Azure u **oblak** način rada.

        azure config mode arm
        azure group deployment create 

- Vrsta resursa ne uključuje **(klasični)** u nazivu. Sljedeća slika prikazuje vrstu kao **račun za pohranu**.

    ![web-aplikacije](./media/resource-manager-deployment-model/resource-manager-type.png)

## <a name="classic-deployment-characteristics"></a>Uvođenje klasičnog značajke

Uvođenje klasičnog modela mogu znati i kao model za upravljanje servisom.

Resursa stvorene u modelu uvođenje klasičnog zajednički koristiti sljedeće značajke:

- Putem [klasične portal](https://manage.windowsazure.com)

     ![Klasični portal](./media/resource-manager-deployment-model/classic-portal.png)

     Ili Azure portal i odredite **Klasični** implementacije (za računalnim, za pohranu i povezivanje s mrežom).

     ![Klasični implementacije](./media/resource-manager-deployment-model/select-classic.png)

- Stvara se kroz verziju Upravljanje servisom Azure PowerShell cmdleti. Naredba nazivi imaju oblik *Glagolski AzureNoun*.

        New-AzureVM 

- Stvara putem [Servisa upravljanje REST API -JA](https://msdn.microsoft.com/library/azure/ee460799.aspx) za OSTALE operacije.
- Stvoriti pomoću naredbe Azure EŽA u **asm** način rada.

        azure config mode asm
        azure vm create 

- Vrsta resursa u nazivu sadrži **(klasični)** . Sljedeća slika prikazuje vrstu kao **račun za pohranu (klasični)**.

    ![Klasični vrsta](./media/resource-manager-deployment-model/classic-type.png)

Koristite Azure portal za upravljanje resursima stvorene putem klasične implementacije.

## <a name="changes-for-compute-network-and-storage"></a>Promjene za računalnim, mreže i prostora za pohranu

Sljedeći dijagram prikazuje računalnim, mreže i pohranu resursi implementiran putem upravitelja resursa.

![Voditelj resursa arhitekture](./media/virtual-machines-azure-resource-manager-architecture/arm_arch3.png)

Imajte na umu sljedeće odnose između resurse:

- Svi resursi postoje grupu resursa.
- Virtualnog računala ovisi o određenim prostora za pohranu računa definirano u davatelja resursa za pohranu za pohranu njegov diskova u spremište blobova (obavezno).
- Virtualnog računala odnosi se na određene NIC definiran u davatelja resursa na mreži (obavezno) i raspoloživost postavljanje definirani u davatelja resursa računalnim (neobavezno).
- NIC-a reference virtualnog računala dodijeljene IP adrese (obavezno), podmreže virtualne mreže za virtualnog računala (obavezno) i (nije obavezno) mreže sigurnosne grupe.
- Podmreže unutar virtualne mreže odnosi se na mreži sigurnosne grupe (neobavezno).
- Instance raspoređivača opterećenja reference pozadinskog skup IP adrese koje obuhvaćaju NIC virtualnog računala (nije obavezno) i upućuje na učitavanja opterećenja javna ili privatna IP adresa (neobavezno).

Evo nekoliko komponenti i njihovih odnosa klasični implementacije:

![Klasični arhitekture](./media/virtual-machines-azure-resource-manager-architecture/arm_arch1.png)

Klasični rješenje za hostiranje virtualnog računala obuhvaća:

- Potrebne oblaka servisom koji funkcionira kao spremnik za hostiranje virtualnim strojevima (računalnim). Mrežna kartica (NIC) automatski dao virtualnim strojevima i IP adresom dodijelio Azure. Uz to, servis u oblaku sadrži instancu raspoređivača učitavanja vanjskih, javnu IP adresu i zadane krajnje točke da bi se udaljene radne površine i remote PowerShell promet virtualnim strojevima utemeljen na sustavu Windows i sigurne ljuske (SSH) promet virtualnim strojevima sustavom Linux.
- Potreban prostor za pohranu računa koji pohranjuje VHDs za virtualnog računala, uključujući operacijski sustav diskova privremene i dodatnih podataka (Spremanje).
- Neobavezni virtualne mreže koji funkcionira kao dodatnog kontejnera, stvaranje subnetted strukture i odrediti na kojem se nalazi virtualnog računala podmreži (mreža).

U sljedećoj tablici opisane promjene u način na koji rade računalnim, mreže i pohranu davatelji resursa:

 Stavke | Klasični | Voditelj resursa
 ---|---|---
| Servis u oblaku za virtualnim strojevima |  Servis u oblaku je spremnik za čuvanje virtualnim strojevima koji zahtijevaju dostupnost od platforme i opterećenja. | Servis u oblaku više nije objekt potrebne za stvaranje virtualnog računala pomoću novog modela. |
| Virtualne mreže | Virtualne mreže nije obavezna za virtualnog računala. Ako je uključena, virtualne mreže ne možete uvesti pomoću upravitelja resursa. | Virtualnog računala potreban je virtualne mreže koji je uveden s Voditelj resursa. |
| Računi za pohranu | Virtualnog računala potreban je račun za pohranu koji pohranjuje VHDs za operacijski sustav diskova privremene i dodatne podatke. | Virtualnog računala potreban je račun za pohranu za pohranu njegov diskova u spremište blobova platforme. |
| Skupovi dostupnosti | Dostupnost platformi je označen konfiguriranja isti "AvailabilitySetName" na virtualnim računalima. Najveći broj od kvara domena je 2. | Postavljanje dostupnosti je resurs koji prikazuje Microsoft.Compute davatelja usluga. Virtualnim strojevima koje zahtijevaju visoke dostupnosti moraju biti obuhvaćene postavljanje dostupnosti. Najveći broj od kvara domena sada je 3. |
| Afinitet grupe | Grupa afinitet su potrebne za stvaranje virtualne mreže. Međutim, uz Uvod u regionalnim virtualne mreže, koji nije potrebno više. |Da biste pojednostavnili, grupe afinitet pojam ne postoji u API-ji izložen putem upravitelja za Azure resursa. |
| Za ujednačavanje opterećenja    | Stvaranje servis u Oblaku nudi implicitno opterećenja virtualnim strojevima implementiran. | Raspoređivača opterećenja je resurs koji prikazuje Microsoft.Network davatelja. Primarni mrežno sučelje virtualnih računala koje treba rasporediti opterećenje treba pozivate raspoređivača opterećenja. Učitavanje Balancers može biti interni ili vanjski. S instancom raspoređivača opterećenja reference pozadinskog skup IP adrese koje obuhvaćaju NIC virtualnog računala (nije obavezno) i upućuje na učitavanja opterećenja javna ili privatna IP adresa (neobavezno). [Dodatne informacije.](../articles/resource-groups-networking.md) |
|Virtualna IP adresa | Kada je VM dodaje se na servis u oblaku, servise u oblaku se zadani VIP (Virtualna IP adresa). Virtualna IP adresa je adresu koja je povezana s implicitno opterećenja.    | Javnu IP adresu je resurs koji prikazuje Microsoft.Network davatelja. Javnu IP adresu može biti statički (Reserved) ili dinamički. Dinamični javnu IP-ovi se mogu dodijeliti raspoređivača opterećenja. Javnu IP-ovi mogu zaštiti pomoću sigurnosne grupe. |
|Rezervirane IP adresa|   Možete rezervirati IP adresa u Azure i povezivanje sa servisom Cloud da biste bili sigurni da je IP adresa ljepljive.   | Moguće je stvoriti javnu IP adresu u načinu rada za "Statične" i nudi istu mogućnost kao "Rezervirane IP adresu u". Statički javnu IP-ovi mogu biti dodijeljena samo raspoređivača opterećenja odmah. |
|Javnu IP adresu (TOČAKA) po VM | Javnu IP adresa mogu biti povezan s VM izravno. | Javnu IP adresu je resurs koji prikazuje Microsoft.Network davatelja. Javnu IP adresu može biti statički (Reserved) ili dinamički. Međutim, samo dinamički javnu IP-ovi se mogu dodijeliti sučelje mreže da biste dobili javnu IP po VM odmah. |
|Krajnje točke| Potreban unos krajnje točke mora biti konfigurirana tako virtualnog računala biti otvoreni gore povezivanje za određeni priključci. Jedna od uobičajenih načini povezivanja virtualnim strojevima gotovo postavljanjem unos krajnje točke. | Ulazna pravila NAT moguće je konfigurirati na učitavanja Balancers da biste postigli istu mogućnost omogućivanja krajnje točke na određene priključke za povezivanje s VMs. |
|Naziv DNS-a| Servis u oblaku dobiti implicitno globalno jedinstveni naziv za DNS-a. Na primjer: `mycoffeeshop.cloudapp.net`. | Nazivi DNS su neobavezni parametri koji se odnose na javnu IP adresu resursa. FQDN je u sljedećem obliku - `<domainlabel>.<region>.cloudapp.azure.com`. |
|Sučelje mreže | Primarni i sekundarni mrežno sučelje i njezina svojstva definirana kao konfiguraciji mreže virtualnog računala. | Mrežno sučelje je resurs koji prikazuje Microsoft.Network davatelja usluga. Životni ciklus mrežno sučelje nije povezana virtualnog računala. Odnosi se virtualnog računala dodijeljene IP adrese (obavezno), podmreže virtualne mreže za virtualnog računala (obavezno) i (nije obavezno) mreže sigurnosne grupe. |

Dodatne informacije o povezivanju virtualne mreže iz modela različitoj implementaciji potražite u članku [povezivanje virtualne mreže iz različitih implementacije modela na portalu](./vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-to-resource-manager"></a>Popis svakodnevnih zadataka klasični upravitelju resursa

Ako ste spremni za migraciju resursa iz uvođenje klasičnog za implementaciju upravljanja resursima, pogledajte:

1. [Tehnički precizno dive na platformi podržane migracije od klasičnog za Azure Voditelj resursa](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
2. [Platforme koje podržava migraciju IaaS resursa od klasičnog za Azure Voditelj resursa](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
3. [Migriranje resursi IaaS od klasičnog Azure resursa upravitelju pomoću komponente PowerShell Azure](./virtual-machines/virtual-machines-windows-ps-migration-classic-resource-manager.md)
4. [Popis svakodnevnih zadataka IaaS resursi klasični Azure resursa upravitelju pomoću EŽA Azure](./virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Najčešća pitanja

**Je li moguće stvoriti virtualnog računala pomoću upravitelja resursa Azure za implementaciju u virtualne mreže ili računa za pohranu stvoren pomoću klasične implementacije?**

Nije podržano. Voditelj resursa Azure ne možete koristiti za implementaciju virtualnog računala u virtualne mreže stvoren pomoću klasične implementacije.

**Je li moguće stvoriti virtualnog računala pomoću upravitelja resursa Azure iz slike korisnika koji je stvoren pomoću upravljanja API-ji servisa Azure?**

Nije podržano. Međutim, možete kopirati datoteke VHD s računa za pohranu koji je stvoren pomoću upravljanja API servisa i njihovo dodavanje u novi račun stvoren pomoću upravitelja za Azure resursa.

**Kakav je učinak na kvote za pretplate?**

Kvota za virtualnim strojevima, virtualne mreže i pohranu račune stvorene pomoću upravitelja resursa Azure razlikuju od drugih kvote. Pretplate dohvaća kvote za stvaranje resurse pomoću novog API-ji. Dodatne informacije o dodatnim kvote [ovdje](../articles/azure-subscription-service-limits.md).

**Možete nastaviti koristite Moje automatiziranog skripte za dodjeljivanje virtualnim strojevima, virtualne mreže i račune za pohranu putem API-ji resursima?**

Automatizacija i skripte koje ste već sastavljali i dalje funkcionirati za postojeće virtualnim strojevima, virtualne mreže koje su stvorene u načinu za upravljanje servisom Azure. Međutim, skripte morate ažurirati namijenjen stvaranju iste resurse kroz MOD upravitelja resursa novu shemu.

**Gdje pronaći Primjeri predložaka Voditelj resursa Azure?**

Opsežan skup predložaka starter pronaći ćete na [Predlošcima za brzi početak rada Azure Voditelj resursa](https://azure.microsoft.com/documentation/templates/).

## <a name="next-steps"></a>Daljnji koraci

- Da biste prošli kroz stvaranje predloška koji definira virtualnog računala, računa za pohranu i virtualne mreže, potražite u članku [Vodič za upravljanje resursima predložak](resource-manager-template-walkthrough.md).
- Naredbe za uvođenje predloška potražite [uvođenja aplikacije pomoću predloška Azure Voditelj resursa](resource-group-template-deploy.md).
