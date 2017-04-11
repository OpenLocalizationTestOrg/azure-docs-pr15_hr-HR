<properties
    pageTitle="Migracija na pohranu Azure Premium | Microsoft Azure"
    description="Migrirati postojeću virtualnim strojevima sa spremištem Premium Azure. Premium prostora za pohranu nudi visokih performansi, najniža Latencija disk podrška za radnih opterećenja li/O-ćete morati usko izvodi na virtualnim strojevima sa sustavom Azure."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="yuemlu"/>


# <a name="migrating-to-azure-premium-storage"></a>Migracija na Azure Premium prostora za pohranu

## <a name="overview"></a>Pregled

Prostor za pohranu Azure Premium nudi visokih performansi, najniža Latencija disk podrška za virtualnim strojevima radi li/O-ćete morati usko radnih opterećenja. Prednost brzine i performansi te diskova može potrajati po migracije diskova VM vaše aplikacije u spremište Premium Azure.

Svrha ovaj vodič je novi korisnici Azure Premium prostora za pohranu bolje Priprema da biste Gladak Prijelaz iz svoje trenutne sustava Premium prostora za pohranu. Vodič adrese tri ključne komponente u taj postupak: 

  - [Plan za migraciju Premium za pohranu](#plan-the-migration-to-premium-storage)
  - [Priprema i kopirajte virtualne tvrdom disku (VHDs) Premium prostora za pohranu](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
  - [Stvaranje Azure virtualnog računala pomoću Premium prostora za pohranu](#create-azure-virtual-machine-using-premium-storage)

Možete migrirati VMs iz druge platforme Azure Premium prostora za pohranu ili migrirati postojeću Azure VMs iz standardnog spremišta Premium prostora za pohranu. Vodič obuhvaća korake za oba dva scenarija. Slijedite korake navedene u odjeljku odgovarajući ovisno o scenariju.

>[AZURE.NOTE] Možete pronaći ćete pregled značajki i cijene Premium prostora za pohranu u spremište Premium: [Visokih performansi prostor za pohranu radnih opterećenja Azure virtualnog računala](storage-premium-storage.md). Preporučujemo da migracije bilo kojem disku virtualnog računala koje je obavezna visoke IOPS Azure Premium spremište radi ostvarivanja najboljih performansi za svoju aplikaciju. Ako na disku potrebna visoka IOPS, možete ograničiti troškove zadržavanjem u standardni prostor za pohranu sprema podatke na disku virtualnog računala na tvrdom disku pogonima (HDDs) umjesto SSDs.

Dovršavanje postupka migracije u potpunosti može zahtijevati dodatne akcije prije i nakon korake navedene u ovom vodiču. Primjeri konfiguraciji virtualne mreže ili krajnje točke ili mijenjanje kod same aplikacije koje možda ćete morati neke isključiti u aplikaciji. Te su akcije jedinstvene za svaku aplikaciju, a trebali biste ih dovršiti te korake navedene u ovom vodiču za cijeli prijelaza s Premium spremište objedinjenog, kao što je moguće.


## <a name="plan-the-migration-to-premium-storage"></a>Plan za migraciju Premium za pohranu

U ovom se odjeljku osigurava spremni, slijedite korake za migraciju u ovom članku i pomaže vam najbolje odluku na vrste VM i Disk.

### <a name="prerequisites"></a>Preduvjeti
- Trebat će vam Azure pretplate. Ako ga nemate, možete stvoriti jedan mjesec [besplatnu probnu](https://azure.microsoft.com/pricing/free-trial/) pretplatu ili posjetite stranicu [Azure cijene](https://azure.microsoft.com/pricing/) za dodatne mogućnosti.
- Izvršavanje cmdleta ljuske PowerShell, trebat će vam modul sustava Microsoft Azure PowerShell. Instalacija točke i instalaciju upute potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .
- Kada namjeravate koristiti Azure VMs sustavom Premium prostora za pohranu, morate koristiti VMs koji podržavaju pohranu Premium. Možete koristiti standardne i pohranu Premium diskova s Premium prostora za pohranu koji podržavaju VMs. Premium prostora za pohranu diskova bit će dostupna s više vrsta VM u budućnosti. Dodatne informacije o sve dostupne Azure VM disk vrste i veličine potražite u članku [veličine za virtualnim strojevima](../virtual-machines/virtual-machines-windows-sizes.md) i [veličine za servise u Oblaku](../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Razmatranja

Programa Azure VM podržava prilaganje više prostora za pohranu Premium diskova tako da se aplikacija može imati do 64 TB prostora za pohranu po VM. S Premium prostor za pohranu, aplikacija možete postići 80,000 IOPS (ulazni i izlazni operacije sekundi) po VM i 2000 MB po drugi propusnost diska po VM s iznimno malo latencies za čitanje operacije. Imate mogućnosti u raznim VMs i diskova. U ovom se odjeljku je da biste lakše pronašli mogućnost koji najbolje odgovara vašem radno opterećenje.

#### <a name="vm-sizes"></a>Veličina VM
Specifikacije veličina Azure VM navedene su u [veličine za virtualnih računala](../virtual-machines/virtual-machines-windows-sizes.md). Pregledajte karakteristike performansi virtualnih računala koje rad s Premium prostora za pohranu, a zatim odaberite najprikladnije veličina VM koji najbolje odgovara vašem radno opterećenje. Provjerite ima li dovoljno propusnosti dostupne na vašem VM na pogon diska promet.


#### <a name="disk-sizes"></a>Veličina na disku
Postoje tri vrste diskova koje je moguće koristiti uz vaše VM i svaka ima određene IOPs i propusnost ograničenja. Uzeti u obzir ta ograničenja kada odaberete vrstu diska za vaše VM skladu s potrebama aplikacije pomoću kapaciteta, performanse i skalabilnost i Vršna učitava.

|Premium prostora za pohranu na disku vrsta|P10|P20|P30|
|:---:|:---:|:---:|:---:|
|Veličina diska|128 GB|512 GB|1024 GB (1 TB)|
|IOPS po na disku|500|2300|5000|
|Propusnost po na disku|100 MB u sekundi|150 MB u sekundi|200 MB u sekundi|

Ovisno o svoje radno opterećenje odrediti ako diskova dodatne podatke potrebne za vaše VM. Nekoliko diskova stalni podataka možete priložiti vaše VM. Ako je potrebno, možete stripe preko diskova da biste povećali kapacitetu i performanse jedinice. (Saznajte što je Podjela Disk [ovdje](storage-premium-storage-performance.md#disk-striping).) Ako ste pruga Premium prostora za pohranu podataka diskova pomoću [Prostori za pohranu][4], potrebno je konfigurirati s jednim stupcem za svaki disk koji se koristi. U suprotnom cjelokupnoj izvedbi glasnoće prugastim može biti manji od očekivanih zbog nejednaki raspodjele prometa preko na diskova. Za Linux VMs uslužni *mdadm* možete koristiti da biste postigli iste. U članku [Konfiguriranje RAID softver na Linux](../virtual-machines/virtual-machines-linux-configure-raid.md) detalje.

#### <a name="storage-account-scalability-targets"></a>Prostor za pohranu računa skalabilnost ciljnih web-mjesta
Računi za pohranu Premium imati sljedeće ciljeve skalabilnost uz [skalabilnost Azure prostora za pohranu i ciljeve](storage-scalability-targets.md). Preduvjeti za aplikaciju prelaze ciljeve skalabilnost računa jednom prostora za pohranu, sastavljanje aplikacije da biste koristili više računa za pohranu i particija podataka u tim računa za pohranu.

|Računu Ukupno kapaciteta|Ukupne propusnosti za račun lokalno suvišnih prostora za pohranu|
|:--|:---|
|Kapacitet na disku: 35TB<br />Snimke kapaciteta: 10 TB|Do 50 gigabits sekundi za ulazni + izlazni|

Dodatne informacije za pohranu Premium specifikacije pogledajte [skalabilnost i ciljeve prilikom korištenja Premium prostora za pohranu](storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

#### <a name="disk-caching-policy"></a>Na disku predmemoriranje pravila
Po zadanom se disk predmemoriranje pravila je *Samo za čitanje* sve podatke Premium diskova i *Čitanja i pisanja* za operacijski sustav disk Premium priložiti u VM. Ta postavka konfiguracije preporučuje se da biste postigli optimalne performanse za aplikaciju sustava IOs. Za pisanje podebljano ili samo za unos podataka diskova (kao što su datoteke zapisnika sustava SQL Server), onemogućiti disk predmemoriju tako da možete postići bolje performanse računala. Postavke predmemorije za postojeće podatke diskova može se ažurirati pomoću [Portala za Azure](https://portal.azure.com) ili parametar *- HostCaching* cmdlet *Skup AzureDataDisk* .

#### <a name="location"></a>Mjesto
Odaberite mjesto na kojem je dostupna Azure Premium prostora za pohranu. Najnovije informacije o mjesta dostupnih potražite u članku [Servisa Azure po regijama](https://azure.microsoft.com/regions/#services) . VMs koji se nalazi u istom području kao račun za pohranu koji služi za pohranu diskova za na VM steći koliko bolje performanse od nalazili u zasebnom regijama.

#### <a name="other-azure-vm-configuration-settings"></a>Ostale postavke za konfiguriranje Azure VM
Prilikom stvaranja programa Azure VM će se tražiti da biste konfigurirali određene postavke VM. Imajte na umu nekoliko postavke rješava za vijek VM, dok je možete izmijeniti ili dodajte druge kasnije. Pregledajte ove postavke konfiguracije Azure VM i provjerite jesu li te konfigurirani pravilno u skladu s potrebama radno opterećenje.

### <a name="optimization"></a>Optimizacija

[Pohranu azure Premium: dizajna za visoke performanse](storage-premium-storage-performance.md) sadrži upute za stvaranje visokih performansi aplikacije koje se koriste za pohranu Premium Azure. Slijedite smjernice u kombinaciji s performansama najbolje prakse dodijeljen tehnologije koristi aplikacija.


## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Priprema i kopirajte virtualne tvrdom disku (VHDs) Premium prostora za pohranu

Sljedeći odjeljak sadrži upute za pripremu VHDs iz vaše VM i kopiranje VHDs Azure prostora za pohranu.

- [Scenarij 1: "mogu se Migracija postojeće Azure VMs Azure Premium prostora za pohranu."](#scenario1)
- [Scenarij 2: "mogu se Migracija VMs s druge platforme Azure Premium prostora za pohranu."](#scenario2)

### <a name="prerequisites"></a>Preduvjeti

Priprema za VHDs za migraciju, morat ćete:

- Azure pretplatu, s računom za pohranu i spremnik u taj račun za pohranu na koji možete kopirati na VHD. Imajte na umu da račun za pohranu odredište može biti standardni prikaz ili prostora za pohranu Premium račun ovisno o svojim potrebama.
- Alat za generalize u VHD Ako planirate stvoriti više instanci VM iz nje. Na primjer, sysprep za Windows ili podmapa člana virtualne kocke sysprep za Ubuntu.
- Alat za prijenos datoteke VHD na račun za pohranu. Potražite u članku [prijenos podataka s Utility za naredbenog retka AzCopy](storage-use-azcopy.md) ili pomoću programa [explorer Azure prostora za pohranu](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Ovaj vodič opisuje kopiranje vaše VHD pomoću alata za AzCopy.

> [AZURE.NOTE] Ako odaberete mogućnost Kopiraj sinkronizirano s AzCopy, postigli optimalne performanse, kopirajte vaše VHD tako da pokrenete jednu od tih alata iz programa Azure VM koji se nalazi u istom području kao račun za pohranu odredište. Ako kopirate u VHD iz programa Azure VM u nekoj drugoj regiji, vaša se može usporiti.
>
> Radi kopiranja veliku količinu podataka putem ograničeni propusnosti, razmislite o [pomoću servisa Azure uvoz/izvoz za prijenos podataka sa spremištem blobova](storage-import-export-service.md); omogućuje prijenos podataka po dostave tvrdom diskovnih pogona Azure podatkovnog centra. Da biste kopirali podatke samo na standardni prostora za pohranu račun pomoću servisa Azure uvoz/izvoz. Kada se podaci nalaze u vašem računu standardne prostora za pohranu, možete koristiti [API Blob Kopiraj](https://msdn.microsoft.com/library/azure/dd894037.aspx) ili AzCopy za prijenos podataka na račun servisa za pohranu premium.
>
> Imajte na umu da Microsoft Azure podržava samo datoteke VHD fixed veličina. VHDX datoteke ili dinamičke VHDs nisu podržane. Ako imate dinamičke VHD, možete je pretvoriti fixed veličine pomoću cmdleta [Pretvori VHD](http://technet.microsoft.com/library/hh848454.aspx) .

### <a name="scenario1"></a>Scenarij 1: "mogu se Migracija postojeće Azure VMs Azure Premium prostora za pohranu."

Ako premještate postojeće VMs Azure, zaustavljanje na VM, Priprema VHDs po vrsti VHD želite, a zatim kopirajte VHD AzCopy ili PowerShell.

Na VM mora biti potpuno dolje da biste migrirali čisto stanje. Pojavit će se na nedostupnost dok se ne dovrši migracije.

#### <a name="step-1-prepare-vhds-for-migration"></a>Korak 1. Priprema VHDs za migraciju

Ako premještate postojeće Azure VMs Premium prostora za pohranu, možda će biti vaša VHD:

- Slika generalizirano operacijskog sustava
- Na disku jedinstveni operacijski sustav
- Podatkovni disk

U nastavku ćemo voditi kroz 3 scenarija za pripremu vaše VHD.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Stvaranje više instanci VM pomoću generalizirano VHD operacijski sustav

Ako prenosite VHD koja će se koristiti za stvaranje više instanci generički Azure VM, najprije morate generalize VHD pomoću programa za sysprep. To se odnosi na VHD koji se nalazi na lokalni ili u oblaku. Sysprep uklanja sve specifične informacije iz sustava VHD.

>[AZURE.IMPORTANT] Stvorite snimku ili sigurnosne kopije vaše VM prije generalizing ga. Izvodi sysprep će prekinuti, a deallocate VM instance. Slijedite korake u nastavku da biste sysprep VHD za s operacijskim Sustavom Windows. Imajte na umu da izvodi naredbu Sysprep ćete morati zatvoriti virtualnog računala. Dodatne informacije o Sysprep potražite u članku [Pregled Sysprep](http://technet.microsoft.com/library/hh825209.aspx) ili [Odjeljku tehničke Reference](http://technet.microsoft.com/library/cc766049.aspx).

1. Otvorite prozor naredbenog retka kao administrator.
2. Unesite sljedeću naredbu da biste otvorili Sysprep:

        %windir%\system32\sysprep\sysprep.exe

3. U alat za pripremu sustava, odaberite unesite sustava – okvir sučelje (OOBE), potvrdite okvir konf, odaberite **zatvaranja**i zatim kliknite **u redu**, kao što je prikazano na slici u nastavku. Sysprep će generalize operacijskog sustava i isključili sustav.

    ![][1]

Da biste postigli VM programa Ubuntu koristite podmapa člana virtualne kocke sysprep da biste postigli iste. [Podmapa člana virtualne kocke sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) dodatne pojedinosti potražite u članku. Pogledajte i neke od Otvori izvor [Linux dodjele na poslužitelju za softver](http://www.cyberciti.biz/tips/server-provisioning-software.html) za drugi operacijski sustavi Linux.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Stvaranje instancu VM pomoću jedinstvenih VHD operacijski sustav

Ako imate aplikacije koje se izvode na VM koji su potrebni stroj određenih podataka, generalize na VHD. VHD za koje nisu GRG generalizirano se može koristiti za stvaranje jedinstvenih Azure VM instance. Na primjer, ako imate kontroler domene na vašem VHD, izvršavanja sysprep će vam tako biti neučinkovite kao kontroler domene. Pregledajte aplikacije koje rade na vašem VM i utjecaj sustavom sysprep ih prije generalizing na VHD.

##### <a name="register-data-disk-vhd"></a>Registrirajte se podatkovni disk VHD

Ako imate diskova podataka u Azure migrirati, provjerite jesu li se isključiti VMs koje koriste tih podataka diskova.

Slijedite korake opisane dolje da biste kopirali VHD za pohranu Premium Azure i registrirati kao na disku dodijeljenu podataka.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Korak 2. Stvaranje odredišta za vaše VHD

Stvaranje računa za pohranu za održavanje vaše VHDs. Prilikom planiranja gdje želite spremiti svoje VHDs Imajte na umu sljedeće:

- Cilj Premium prostora za pohranu računa.
- Mjesto za pohranu račun mora biti jednak kao Premium prostora za pohranu koji podržavaju Azure VMs ćete stvoriti u faza. Nije moguće kopirati na novi račun za pohranu ili namjeravate koristiti isti prostor za pohranu račun koji se temelji na vašim potrebama.
- Kopiranje i spremite ključ računa za pohranu za pohranu računa odredište za sljedeću fazu.

Diskova podatke, možete odabrati da biste zadržali neke diskova podataka u standardne prostora za pohranu računa (na primjer, diskova koji imaju hladnijom prostora za pohranu), ali ne preporučujemo vam premještanje sve podatke za radno opterećenje radni da biste koristili premium prostora za pohranu.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>Korak 3. Kopiranje VHD AzCopy ili PowerShell

Morat ćete pronaći spremnik put i pohranu ključ računa za obradu neku od sljedeće dvije mogućnosti. Spremnik put i pohranu ključ računa moguće je pronaći u **Azure Portal** > **prostora za pohranu**. Spremnik URL će se kao što su "https://myaccount.blob.core.windows.net/mycontainer/".

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Mogućnost 1: Kopiranje VHD s AzCopy (asinkronog kopiranje)

Korištenje AzCopy, možete jednostavno prenijeti u VHD putem Interneta. Ovisno o veličini u VHDs, to može potrajati. Imajte na umu da biste provjerili ingress/izlazne ograničenja vezana uz račun za pohranu koristite tu mogućnost. Dodatne informacije potražite u [skalabilnost Azure prostora za pohranu i ciljeve](storage-scalability-targets.md) .

1. Preuzmite i instalirajte AzCopy odavde: [najnoviju verziju AzCopy](http://aka.ms/downloadazcopy)
2. Otvorite Azure PowerShell i idite u mapu na kojem je instaliran AzCopy.
3. Koristite sljedeću naredbu da biste kopirali datoteku VHD "Izvora" "Odredište".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Primjer:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd

    Slijede opisi parametara koji se koriste u naredbu AzCopy:

 - * */Izvora: * &lt;izvora&gt;:*** mjesto mape ili URL spremnik za pohranu koji sadrži na VHD.
 - * */SourceKey: * &lt;izvora, račun i ključa&gt;:*** ključ za račun za pohranu za pohranu računa izvora.
 - * */Dest: * &lt;odredište&gt;:*** URL-a za pohranu spremnik da biste kopirali VHD da biste.
 - * */DestKey: * &lt;odredišne račun ključa&gt;:*** ključ za račun za pohranu za pohranu računa odredište.
 - * */Uzorka: * &lt;naziv datoteke&gt;:*** Navedite naziv datoteke VHD da biste kopirali.

Informacije o korištenju alata za AzCopy, potražite u članku [prijenos podataka s AzCopy naredbenog retka uslužni](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Mogućnost 2: Kopiranje VHD sa servisom PowerShell (Synchronized kopiranje)

Možete i kopirati datoteku VHD pomoću cmdleta ljuske PowerShell Start AzureStorageBlobCopy. Da biste kopirali VHD koristite sljedeću naredbu na Azure PowerShell. Zamjena vrijednosti u <> s odgovarajućim vrijednostima iz izvorišne i odredišne računa za pohranu. Da biste koristili tu naredbu, morate imati spremnika koji se naziva vhds na vašem računu za pohranu odredište. Ako je spremnik ne postoji, stvorite ga prije pokretanja naredbe.

    $sourceBlobUri = <source-vhd-uri>

    $sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

    $destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

    Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext

Primjer:

    C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

    C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

    C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

    C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext

### <a name="scenario2"></a>Scenarij 2: "mogu se Migracija VMs s druge platforme Azure Premium prostora za pohranu."

Ako premještate VHD iz nije Azure oblaka za pohranu za Azure, najprije morate izvesti na VHD lokalnom direktoriju. Imati izvorni potpuni put datoteke lokalni direktorij VHD pohrane pri ruci, a onda odaberete AzCopy da biste prenijeli na Azure prostora za pohranu.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>Korak 1. Izvoz VHD u lokalni direktorij

##### <a name="copy-a-vhd-from-aws"></a>Kopirajte na VHD iz AWS

1. Ako koristite AWS, izvezite instancu EC2 VHD u programa Amazon S3 grupe. Slijedite korake opisane u izvoz instance EC2 Amazon da biste instalirali alat za Amazon EC2 sučelje naredbenog retka (EŽA) i pokrenite naredbu Stvori-instancu-izvoz-zadatak da biste izvezli instancu EC2 VHD datoteka potražite u dokumentaciji Amazon. Ne zaboravite da biste koristili **VHD** DISK & #95; SLIKA & #95; Varijabla OBLIKOVANJE kad pokrećete naredbu **Stvori-instancu-izvoz-zadatak** . Izvezenu datoteku VHD sprema se u grupe Amazon S3 odrediti tijekom tog postupka.

        aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
        --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX

2. Preuzmite datoteku VHD iz grupe S3. Odaberite datoteku VHD, zatim **Akcije** > **Preuzimanje**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Kopirajte na VHD iz drugih oblak koji nisu Azure

Ako premještate VHD iz nije Azure oblaka za pohranu za Azure, najprije morate izvesti na VHD lokalnom direktoriju. Kopirajte izvorni potpuni put datoteke lokalni direktorij pohranjuju VHD.

##### <a name="copy-a-vhd-from-on-premise"></a>Kopirajte na VHD iz na lokaciji

Ako premještate VHD iz lokalnog okruženja, morate dovršeno izvorni put pohranjuju VHD. Izvorni put može biti na mjesto ili datoteka zajednička mapa na poslužitelju.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Korak 2. Stvaranje odredišta za vaše VHD

Stvaranje računa za pohranu za održavanje vaše VHDs. Prilikom planiranja gdje želite spremiti svoje VHDs Imajte na umu sljedeće:

- Račun za pohranu cilj može biti standardno ili premium prostora za pohranu ovisno o svojim potrebama aplikacije.
- Područje račun za pohranu mora biti jednak kao Premium prostora za pohranu koji podržavaju Azure VMs ćete stvoriti u faza. Nije moguće kopirati na novi račun za pohranu ili namjeravate koristiti isti prostor za pohranu račun koji se temelji na vašim potrebama.
- Kopiranje i spremite ključ računa za pohranu za pohranu računa odredište za sljedeću fazu.

Preporučujemo vam premještanje sve podatke za radno opterećenje proizvodnje da biste koristili premium prostora za pohranu.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>Korak 3. Prijenos na VHD Azure prostora za pohranu

Sad kad ste na VHD u lokalnom direktoriju, možete koristiti AzCopy ili AzurePowerShell da biste prenijeli datoteku .vhd na Azure prostora za pohranu. Obje mogućnosti su navedene ovdje:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Mogućnost 1: Korištenje Azure PowerShell Dodaj-AzureVhd da biste prenijeli datoteku .vhd

    Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>

Primjer <Uri> možda **_"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"_**. Primjer <FileInfo> možda **_"C:\path\to\upload.vhd"_**.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Mogućnost 2: Korištenje AzCopy za prijenos datoteke .vhd

Korištenje AzCopy, možete jednostavno prenijeti u VHD putem Interneta. Ovisno o veličini u VHDs, to može potrajati. Imajte na umu da biste provjerili ingress/izlazne ograničenja vezana uz račun za pohranu koristite tu mogućnost. Dodatne informacije potražite u [skalabilnost Azure prostora za pohranu i ciljeve](storage-scalability-targets.md) .

1. Preuzmite i instalirajte AzCopy odavde: [najnoviju verziju AzCopy](http://aka.ms/downloadazcopy)
2. Otvorite Azure PowerShell i idite u mapu na kojem je instaliran AzCopy.
3. Koristite sljedeću naredbu da biste kopirali datoteku VHD "Izvora" "Odredište".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Primjer:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd

    Slijede opisi parametara koji se koriste u naredbu AzCopy:

 - * */Izvora: * &lt;izvora&gt;:*** mjesto mape ili URL spremnik za pohranu koji sadrži na VHD.
 - * */SourceKey: * &lt;izvora, račun i ključa&gt;:*** ključ za račun za pohranu za pohranu računa izvora.
 - * */Dest: * &lt;odredište&gt;:*** URL-a za pohranu spremnik da biste kopirali VHD da biste.
 - * */DestKey: * &lt;odredišne račun ključa&gt;:*** ključ za račun za pohranu za pohranu računa odredište.
 - **/BlobType: stranice:** Određuje je li odredište blob stranice.
 - * */Uzorka: * &lt;naziv datoteke&gt;:*** Navedite naziv datoteke VHD da biste kopirali.

Informacije o korištenju alata za AzCopy, potražite u članku [prijenos podataka s AzCopy naredbenog retka uslužni](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Druge mogućnosti za prijenos u VHD

Na račun za pohranu na jedan od sljedećih srednje možete prenijeti na VHD:

- [Kopiraj Azure spremište blobova platforme API-JA](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure prostora za pohranu Explorer prijenos blob-ova](https://azurestorageexplorer.codeplex.com/)
- [Prostor za pohranu uvoz/izvoz servisa REST API Reference](https://msdn.microsoft.com/library/dn529096.aspx)

>[AZURE.NOTE] Preporučujemo da putem servisa za uvoz/izvoz ako Procijenjena prijenos vrijeme je više od sedam dana. [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) možete koristiti za procjenu vrijeme od jedinice veličine i prijenos podataka.
>
> Da biste kopirali standardni prostora za pohranu račun može se koristiti uvoz/izvoz. Morat ćete kopirali standardni prostora za pohranu premium prostora za pohranu račun pomoću alata kao što je AzCopy.


## <a name="create-azure-virtual-machine-using-premium-storage"></a>Stvaranje VMs Azure pomoću Premium prostora za pohranu

Nakon što u VHD prenijeti ili kopirali račun željeni prostora za pohranu, slijedite upute u ovom odjeljku da biste registrirali na VHD kao sliku OS ili OS diska ovisno o scenariju i iz nje stvorite VM instance. Podatkovni disk VHD se mogu priložiti u VM nakon stvaranja. Ogledna skripta za migraciju navedeni su na kraju ovaj odjeljak. U ovom jednostavnu skriptu ne odgovara svim situacijama. Možda ćete morati ažurirati skripte u skladu s određenim scenariju. Ako Ova skripta odnosi se na vašu situaciju potražite ispod [Skripti za migraciju odgovora uzorka](#a-sample-migration-script).

### <a name="checklist"></a>Kontrolni popis

1.  Pričekajte dok se sve diskova VHD kopiranje je dovršiti.
2.  Provjerite je li za pohranu Premium je dostupna u regiji premještate u.
3.  Odlučite nove nizove VM namjeravate koristiti. Trebala bi biti pohranu Premium koje je moguće povezati, a veličina treba ovisno o dostupnosti u regiji i ovisno o vašim potrebama.
4.  Odlučite točno veličina VM će koristiti. Veličina VM mora biti dovoljna za podršku broj podataka diskova imate. Npr. Ako imate 4 diskova podataka, na VM mora imati jezgri 2 ili više. Uzmite u obzir obrade power, memorije i mora propusnost mreže.
5.  Stvorite račun za pohranu Premium u regiji cilj. Ovo je račun će se koristiti za nove VM.
6.  Imati trenutni VM detalje pri ruci, uključujući popis diskova i odgovarajuće VHD blob-ova.

Priprema aplikacija za nedostupnost. Da biste učinili clean migracije, imate da biste zaustavili sve obrade trenutni sustav. Samo pa je možete nabaviti dosljedan stanje koje možete preseliti na novu platformu. Nedostupnost trajanje ovisi o količinu podataka u diskova će se migrirati.

>[AZURE.NOTE] Ako stvarate programa VM resursima Azure s specijalizirane VHD diska, pogledajte [Ovaj predložak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) za implementaciju VM resursima pomoću postojeće diska.

### <a name="register-your-vhd"></a>Registrirajte se vaš VHD

Da biste stvorili na VM iz OS VHD ili na disku podataka priložite novi VM, prvo morate registrirati ih. Slijedite korake u nastavku ovisno o scenariju vaše VHD.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>GRG generalizirano VHD operacijski sustav da biste stvorili više instanci Azure VM

Nakon prijenosa slika generalizirano OS VHD na račun za pohranu registrirati kao **Azure VM slike** tako da stvorite jedan ili više instanci VM iz nje. Koristite sljedeće Cmdlete ljuske PowerShell za registraciju na VHD kao slika Azure VM OS. Unijeti URL dovršeno spremnik gdje je VHD kopiran u.

    Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows

Kopiranje i spremiti naziv nove slike Azure VM. U gornjem primjeru je *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Jedinstveni VHD operacijski sustav da biste stvorili instancu Azure VM

Nakon prijenosa jedinstveni VHD OS na račun za pohranu registrirati kao **Azure OS Disk** tako da stvorite VM instancu iz nje. Da biste registrirali vaše VHD kao na Disk OS Azure pomoću tih cmdleta ljuske PowerShell. Unijeti URL dovršeno spremnik gdje je VHD kopiran u.

    Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"

Kopiranje i spremiti naziv novog diska Azure OS. U gornjem primjeru je *OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Podaci na disku VHD priloženi nove instance Azure VM

Nakon prijenosa na disku VHD podataka na račun za pohranu registrirati kao na Disk Azure podataka tako da ga možete priložiti novi DS nizu, DSv2 niz ili Oznaka niz Azure VM instance.

Pomoću tih cmdleta ljuske PowerShell registrirati svoje VHD kao na Disk Azure podataka. Unijeti URL dovršeno spremnik gdje je VHD kopiran u.

    Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"

Kopiranje i spremiti naziv novog diska Azure podataka. U gornjem primjeru je *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Stvaranje VM za koje je moguće povezati Premium prostora za pohranu

Jednom slika OS ili registrirane OS disk, stvorite novi niz DS, DSv2 niz ili VM Oznaka niz. Koristite operacijski sustav sliku ili naziv diska operacijski sustav koji ste registrirali. Odaberite vrstu VM od Premium prostora za pohranu sloju. U primjeru u nastavku ćemo koriste veličina VM *Standard_DS2* .

>[AZURE.NOTE] Ažurirajte veličina diska da biste bili sigurni odgovara vašem kapaciteta i preduvjeti performanse i veličine Azure slobodnog.

Slijedite dolje da biste stvorili novi VM cmdleta ljuske PowerShell korak po korak. Prvo postavljanje uobičajenih parametara:

    $serviceName = "yourVM"
    $location = "location-name" (e.g., West US)
    $vmSize ="Standard_DS2"
    $adminUser = "youradmin"
    $adminPassword = "yourpassword"
    $vmName ="yourVM"
    $vmSize = "Standard_DS2"

Prvo, stvorite neki servis u oblaku u kojem će se nalaze na novu VMs.

    New-AzureService -ServiceName $serviceName -Location $location

Nakon toga ovisno o scenariju, stvoriti instancu Azure VM s OS slike ili OS Disk koju ste registrirali.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>GRG generalizirano VHD operacijski sustav da biste stvorili više instanci Azure VM

Stvorite jedan ili više novi DS niz Azure VM instance pomoću **Azure OS slike** koju ste registrirali. Navedite taj naziv OS slika u konfiguraciji VM prilikom stvaranja novog VM kao što je prikazano u nastavku.

    $OSImage = Get-AzureVMImage –ImageName "OSImageName"

    $vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

    Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

    New-AzureVM -ServiceName $serviceName -VM $vm

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Jedinstveni VHD operacijski sustav da biste stvorili instancu Azure VM

Stvorite novu DS niz Azure VM instancu pomoću **Diska Azure OS** koju ste registrirali. Odredite taj naziv diska OS u konfiguraciji VM prilikom stvaranja novog VM kao što je prikazano u nastavku.

    $OSDisk = Get-AzureDisk –DiskName "OSDisk"

    $vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

    New-AzureVM -ServiceName $serviceName –VM $vm

Navedite ostale informacije Azure VM, kao što je servis u oblaku, regija, računa za pohranu, postavljanje dostupnosti i predmemoriranja pravila. Imajte na umu da instancu VM mora biti Suradnja nalazi povezana operacijski sustav ili diskova podataka da bi se odabrane oblak račun servisa, regije i pohranu svi moraju biti na istom mjestu kao podlozi VHDs te diskova.

### <a name="attach-data-disk"></a>Prilaganje podatkovni disk

Na kraju, ako ste registrirali podatkovni disk VHDs, ih priložite u novi prostor za pohranu Premium instaliranih Azure VM.

Koristite sljedeći cmdlet ljuske PowerShell podatkovni disk pridružiti novi VM i odredite predmemoriranja pravila. U primjeru u nastavku je predmemoriranja pravilo postavljeno na *samo za čitanje*.

    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

    Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

    Update-AzureVM  -VM $vm

>[AZURE.NOTE] Mogu postojati i dodatni koraci potrebne za podršku aplikaciji koja je nije moguće pokriven ovaj vodič.

### <a name="checking-and-plan-backup"></a>Provjera i planiranje sigurnosnog kopiranja

Kada je novi VM s radom, pristupati s istom id za prijavu i lozinke kao izvornu VM i provjerite je li taj funkcionira sve očekivani. Sve postavke, uključujući prugastim količine bi koje su prisutne u novi VM.

Posljednji korak je planirati sigurnosnu kopiju i održavanje raspored za novi VM na temelju potreba aplikacije.

### <a name="a-sample-migration-script"></a>Ogledna skripta za migraciju

Ako imate više VMs će se migrirati, automatizacija putem skripte komponente PowerShell će biti koristan. Slijedi primjer skripte za automatiziranje migracije na VM. Napomena ispod skripte je samo primjer i postoji nekoliko pretpostavke o trenutnom VM diskova. Možda ćete morati ažurirati skripte u skladu s određenim scenariju.

Na pretpostavke su:

- Stvarate klasičnih Azure VMs.
- Su u isti račun za pohranu i isti spremnik diskova OS izvora i diskova s izvorom podataka. Ako OS diskova i diskova podataka nisu na istom mjestu, možete koristiti AzCopy ili Azure PowerShell da biste kopirali VHDs putem računa za pohranu i spremnike. Odnosi se na prethodni korak: [Kopiranje VHD AzCopy ili PowerShell](#copy-vhd-with-azcopy-or-powershell). Uređivanje ovu skriptu da bi odgovarao scenariju je nešto drugo, no preporučujemo korištenje AzCopy ili PowerShell Budući da je lakše i brže.

Skripta za automatizaciju navedene u nastavku. Zamjena teksta s vašim podacima i ažurirati skripte u skladu s određenim scenariju.

>[AZURE.NOTE] Korištenje postojeće skripte sačuvati mrežnoj konfiguraciji izvora VM. Morat ćete ponovno config mrežne postavke na vašem migriranim VMs.

    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED “AS IS” WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location “Southeast Asia”

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you’d like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location

#### <a name="optimization"></a>Optimizacija

Vaša trenutna konfiguracija VM moguće je prilagoditi Konkretno za rad s standardne diskova. Na primjer, da biste povećali performanse pomoću mnogo diskova u prugastim jedinicu. Ako, na primjer, umjesto korištenja 4 diskova odvojeno na Premium prostora za pohranu, možda ćete moći optimizira trošak pojavljuju jedan disk. Optimizacije kao što su ova potrebe rukovati na pojedinačno i zahtijevaju prilagođene korake nakon migracije. Osim toga, imajte na umu da ovaj postupak možda neće dobro funkcionirati za baze podataka i aplikacije koje ovise o rasporedu disk definirano u postavi.

##### <a name="preparation"></a>Priprema

1.  Dovršite jednostavne migracije opisan u prethodnom odjeljku. Optimizacije provest će se na novi VM nakon migracije.
2.  Definiranje nove veličine diska koja su potrebna za konfiguraciju optimizirana.
3.  Odredite mapiranje trenutni diskova/jedinica za nove specifikacije disk.

##### <a name="execution-steps"></a>Upute za izvođenje

1.  Stvaranje novih diskova s desnog veličine na pohranu VM Premium.
2.  Prijavite se na VM i kopiranje podataka iz trenutne jedinice novi disk koji se preslikava tu jedinicu. To za sve trenutne količine koje je potrebno da biste mapirali novi disk.
3.  Zatim Promjena postavki aplikacije da biste prešli na novu diskova i odvajanje stare količine.

Za ugađanje aplikacije za bolje performanse diska, pogledajte [Optimiziranje performanse računala](storage-premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Migracija aplikacije

Baza podataka i drugim aplikacijama za složene može zahtijevati posebne korake onako kako su definirana davatelj aplikacije za migraciju. Potražite u dokumentaciji odgovarajući aplikacije. Npr. obično možete prenositi baze podataka pomoću sigurnosnog kopiranja i vraćanja.

## <a name="next-steps"></a>Daljnji koraci

Potražite u sljedećim resursima za određene scenariji za migriranje virtualnog računala:

- [Migriranje Azure virtualnim strojevima između računa za pohranu](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
- [Stvaranje i prijenos VHD za poslužitelj sustava Windows Azure.](../virtual-machines/virtual-machines-windows-classic-createupload-vhd.md)
- [Stvaranje i prijenos virtualne tvrdi Disk koji sadrži Linux operacijski sustav](../virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md)
- [Migriranje virtualnim strojevima sa Amazon AWS za Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Osim toga, pogledajte sljedeće resurse da biste saznali više o Azure prostora za pohranu i virtualnim računalima sustava Azure:

- [Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/)
- [Azure virtualnim strojevima](https://azure.microsoft.com/documentation/services/virtual-machines/)
- [Prostor za pohranu Premium: Visokih performansi prostor za pohranu radnih opterećenja Azure virtualnog računala](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
