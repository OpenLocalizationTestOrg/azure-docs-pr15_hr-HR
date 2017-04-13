<properties
    pageTitle="Pregled virtualnim strojevima sa sustavom Windows | Microsoft Azure"
    description="Saznajte više o stvaranju pravila i upravljanju virtualnim računalima sustava Windows u Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="davidmu"/>

# <a name="overview-of-windows-virtual-machines-in-azure"></a>Pregled virtualnim računalima sustava Windows u Azure

Azure virtualnim strojevima (VM) je jedan od nekoliko vrsta [na zahtjev, skalabilni potrebnom resursi](../app-service-web/choose-web-site-cloud-service-vm.md) koji nudi Azure. Obično je da se na VM odabrati kada je potrebno više kontrole nad računalno okruženje nego što se mogućnosti nude. U ovom se članku daje informacije o što razmotrite prije stvaranja na VM, kako stvoriti i kako upravljati.

Programa Azure VM pruža fleksibilnost virtualizacije bez potrebe za kupnju i održavanje fizički hardverski koja ga pokreće. Međutim, i dalje morate da biste zadržali na VM tako da obavljaju zadatke, kao što je konfiguriranje zakrpa i instalaciju softvera koja se izvršava na njemu.

Azure virtualnih računala može se koristiti na razne načine. Primjeri su:

- **Razvoj i testiranje** – ponuda za Azure VMs na brz i jednostavan način da biste stvorili računalo s određenim konfiguracijama potreban kod i testiranje aplikacije.
- **Aplikacija u oblak** – jer je zahtjev za svoju aplikaciju možete mijenjaju, možda bi Ekonomske osjećaj za izvođenje na VM u Azure. Platite dodatni VMs kada budu potrebne i isključite ih kada znate.
- **Podatkovnim centrom prošireno** – virtualnim strojevima u Azure virtualne mreže lako može se povezati s mreže vaše tvrtke ili ustanove.

Broj VMs koji koristi vaše aplikacije mogu mijenjati veličinu prema gore ili odlazi na neku drugu potreban je da bi odgovarao vašim potrebama.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Što je potrebno da razmislite o prije stvaranja na VM?

Postoje uvijek više [dizajna okolnosti](virtual-machines-windows-infrastructure-virtual-machine-guidelines.md) pri izgradnje infrastruktura za aplikacije u Azure. Ove aspekte na VM su važne razmislite o prije nego što počnete:

- Nazive resursa u aplikaciju
- Mjesto pohrane resursi
- Veličina u VM
- Maksimalan broj VMs koje je moguće stvoriti
- Operacijski sustav koji se izvodi na VM
- Konfiguriranje VM nakon pokretanja
- Povezani resursi koji je potrebno u VM

### <a name="naming"></a>Dodjela naziva

Virtualnog računala ima [naziv](virtual-machines-windows-infrastructure-naming-guidelines.md) dodijeljen, a ima naziv računala konfigurirana kao dio operacijski sustav. Naziv je VM može biti 15 znakova.

Ako koristite Azure diska operacijski sustav, naziv računala i naziv virtualnog računala jednake. Ako ste [prenijeli i koristite vlastitu sliku](virtual-machines-windows-upload-image.md) koja sadrži prethodno konfigurirane operacijski sustav, a zatim ga koristiti za stvaranje virtualnog računala, nazive može razlikovati. Preporučujemo da kada Prenesite datoteku slike, unesete naziv računala u operacijskom sustavu, a isti naziv virtualnog računala.

### <a name="locations"></a>Mjesta

Sve resursa stvorene Azure su raspodijeliti više [zemljopisnim područjima](https://azure.microsoft.com/regions/) diljem svijeta. Obično područje zove **mjesto** prilikom stvaranja na VM. VM, mjesto određuje se pohranjuju virtualne tvrdom disku.

U ovoj su tablici navedene neke načine možete dohvatiti popis dostupnih mjesta.

| Način | Opis |
| ------ | ----------- |
| Portal za Azure | Kada stvorite na VM, odaberite mjesto na popisu. |
| Azure PowerShell | Koristite naredbu [Get-AzureRmLocation](https://msdn.microsoft.com/library/mt619449.aspx) . |
| REST API-JA | Pomoću [popisa mjesta](https://msdn.microsoft.com/library/dn790540.aspx) operacije. |

### <a name="vm-size"></a>Veličina VM

[Veličina](virtual-machines-windows-sizes.md) VM koju koristite određen radno opterećenje koji želite pokrenuti. Zatim odaberite željenu veličinu određuje čimbenicima kao što su obrade kapaciteta power, memorije i prostora za pohranu. Azure nudi raznih veličina za podršku razne vrste koristi.

Azure naknada za [svaki sat cijena](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) na temelju veličine i operacijski sustav na VM. Djelomično sata Azure naknada samo za vrijeme u minutama koristi. Prostor za pohranu je cjenovnom i naplatiti zasebno.

### <a name="vm-limits"></a>Ograničenja VM

Pretplata obuhvaća zadane [ograničenja](../azure-subscription-service-limits.md) na mjesto na kojem se može utjecati na implementaciju mnoge VMs projekta. Trenutno ograničenje na temelju po pretplate je 20 VMs po regijama. Ograničenja možete se zaokružuje prema arhiviranja podršku ulaznice traži povećava.

### <a name="operating-system-disks-and-images"></a>Operacijski sustav diskova i slike

Virtualnim strojevima koristite [virtualne tvrdom disku (VHDs)](virtual-machines-windows-about-disks-vhds.md) za pohranu njihove operacijskim sustavima (OS) i podataka. VHDs se koriste za slike možete odabrati da biste instalirali na OS. 

Azure nudi brojne [trgovine slike](https://azure.microsoft.com/marketplace/virtual-machines/) želite koristiti s različitim verzijama i vrste operacijski sustavi Windows Server. Slika trgovine prepoznaju se po publisher slike, ponuda, sku i verzija (obično verzija je navedena kao najnoviji). 

Ova tablica sadrži nekoliko načina na koje možete pronaći podatke za sliku.

| Način | Opis |
| ------ | ----------- |
| Portal za Azure | Vrijednosti umjesto vas automatski su navedeni kad odaberete sliku koja će se koristiti. |
| Azure PowerShell | [Get-AzureRMVMImagePublisher](https://msdn.microsoft.com/library/mt603484.aspx) -mjesto "mjesto"<BR>[Get-AzureRMVMImageOffer](https://msdn.microsoft.com/library/mt603824.aspx) -mjesto "mjesto"-"publisherName" u programu Publisher<BR>[Get-AzureRMVMImageSku](https://msdn.microsoft.com/library/mt619458.aspx) -mjesto "mjesto"-Publisher "publisherName"-nude "offerName" |
| REST API-ji | [Izdavači popisa slika](https://msdn.microsoft.com/library/mt743702.aspx)<BR>[Slika popisa nudi](https://msdn.microsoft.com/library/mt743700.aspx)<BR>[Popis slika SKU-ove](https://msdn.microsoft.com/library/mt743701.aspx) |

Možete odabrati da biste [prenijeli i koristite vlastitu sliku](virtual-machines-windows-upload-image.md) , a kada to učinite, ne koristi naziv izdavača, ponude i SKU-om.

### <a name="extensions"></a>Proširenja

VM [proširenja](virtual-machines-windows-extensions-features.md) dati putem konfiguracije objave implementacije i automatizirane zadatke na VM dodatne mogućnosti.

Uobičajeni zadaci je moguće napraviti pomoću proširenja:

- **Pokretanje prilagođene skripte** – [Prilagođene skripte proširenje](virtual-machines-windows-extensions-customscript.md) olakšava možete konfigurirati radnih opterećenja na na VM tako da pokrenete skriptu kada je na VM dodjeli.
- **Uvođenja i upravljanju konfiguracijama** – [PowerShell želji stanje konfiguracije (DSC) nastavak](virtual-machines-windows-extensions-dsc-overview.md) će vam pomoći DSC na na VM da biste upravljali konfiguraciju i okruženja.
- **Prikupljanje podataka Dijagnostika** – [Azure Dijagnostika proširenje](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) olakšava možete konfigurirati VM da biste prikupili podatke za dijagnostiku koje je moguće koristiti za praćenje stanja aplikacije.

### <a name="related-resources"></a>Povezani resursi

Resursi u ovoj tablici koji se koristi u VM i morate postoji ili se može stvoriti kada se stvara u VM.

| Resurs | Obavezno | Opis |
| -------- | -------- | ----------- |
| [Grupa resursa](../azure-resource-manager/resource-group-overview.md) | Da | Na VM moraju se nalaziti u grupu resursa. |
| [Račun za pohranu](../storage/storage-create-storage-account.md) | Da | Na VM potreban račun za pohranu za pohranu virtualne tvrdi disk. |
| [Virtualne mreže](../virtual-network/virtual-networks-overview.md) | Da | Na VM mora biti član virtualne mreže. |
| [Javna IP adresa](../virtual-network/virtual-network-ip-addresses-overview-arm.md) | ne | Na VM može imati javnu IP adresa dodijeljena za daljinski pristup. |
| [Mrežno sučelje](../virtual-network/virtual-network-network-interface-overview.md) | Da | Na VM mora mrežno sučelje za komunikaciju u mreži. |
| [Diskova podataka](virtual-machines-windows-attach-disk-portal.md) | ne | Na VM mogu sadržavati podatke diskova da biste proširili mogućnosti prostora za pohranu. |

## <a name="how-do-i-create-my-first-vm"></a>Kako stvoriti Moje prvi VM?

Imate nekoliko mogućnosti za stvaranje vaše VM. Odabir koji unesete ovisi o okruženju na kojoj se nalazite. 

U ovoj su tablici navedene informacije za početak rada i stvaranje vaše VM.

| Način | Članak |
| ------ | ------- |
| Portal za Azure | [Stvaranje virtualnog računala sa sustavom Windows pomoću portala](virtual-machines-windows-hero-tutorial.md) |
| Predlošci | [Stvaranje virtualnog računala za Windows s predloškom Voditelj resursa](virtual-machines-windows-ps-template.md) |
| Azure PowerShell | [Stvaranje Windows VM pomoću komponente PowerShell](virtual-machines-windows-ps-create.md) |
| Klijent SDK-ovi | [Implementacija resurse za Azure pomoću C#](virtual-machines-windows-csharp.md) |
| REST API-ji | [Stvaranje ili ažuriranje na VM](https://msdn.microsoft.com/library/mt163591.aspx) |

Nadam se da nikada ne događa, ali ponekad nešto pošlo po redu. Ako se ova situacija vama, pogledajte informacije u [Voditelj resursa za otklanjanje poteškoća s implementacije problemi sa stvaranjem virtualnog računala za Windows u Azure](virtual-machines-windows-troubleshoot-deployment-new-vm.md).

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Kako upravljati VM koji sam stvorio?

VMs može upravljati alatima za portal utemeljenima na pregledniku naredbenog retka s podrškom za skriptiranje ili izravno putem API-ji. Neke uobičajene upravljanje zadatke koje mogu izvršiti su dohvaćanje podataka o VM, prijavu VM, upravljanje dostupnosti i stvaranje sigurnosne kopije.

### <a name="get-information-about-a-vm"></a>Informacije o na VM

U ovoj su tablici prikazuje neki od načina na koje možete dobiti informacije o na VM.

| Način | Opis |
| ------ | ----------- |
| Portal za Azure | Na izborniku koncentrator kliknite **virtualnim računalima sustava** , a zatim na VM s popisa. Na plohu za na VM, imate pristup pregled informacija postavljanje vrijednosti i nadzor metriku. |
| Azure PowerShell | Informacije o korištenju ljuske PowerShell za upravljanje VMs potražite u članku [Upravljanje virtualnim računalima sustava Azure pomoću upravitelja resursa i PowerShell](virtual-machines-windows-ps-manage.md). |
| REST API-JA | Pomoću operacije [informacije za početak VM](https://msdn.microsoft.com/library/mt163682.aspx) radi dobivanja informacija o na VM. |
| Klijent SDK-ovi | Informacije o korištenju C# za upravljanje VMs potražite u članku [Upravljanje virtualnim računalima sustava Azure pomoću upravitelja resursa Azure i C#](virtual-machines-windows-csharp-manage.md). |

### <a name="log-on-to-the-vm"></a>Prijavite se na VM

Pomoću gumba za povezivanje na portalu za Azure da biste [započeli sesiju udaljene radne površine (RDP)](virtual-machines-windows-connect-logon.md). Može ponekad doći do problema prilikom korištenja daljinske veze. Ako se ova situacija vama, pročitajte članak pomoć u [Otklanjanje poteškoća s veze udaljene radne površine za Azure virtualnog računala sa sustavom Windows](virtual-machines-windows-troubleshoot-rdp-connection.md).

### <a name="manage-availability"></a>Upravljanje dostupnosti

Važno ćete se snaći je kako [visoke osiguraj](virtual-machines-windows-manage-availability.md) aplikacije. Tu konfiguraciju obuhvaća više VMs da biste bili sigurni da barem jedan radi stvaranja.

Implementaciju sustava da biste potvrdili za naše 99.95 VM razinu ugovor o usluzi, morate uvesti dva ili više VMs izvodi svoje radno opterećenje unutar programa [Postavljanje dostupnosti](virtual-machines-windows-infrastructure-availability-sets-guidelines.md). Tu konfiguraciju osigurava vaše VMs su raspodijeliti više domena kvara i uvode se na glavno računalo sa sustavom windows različite održavanja. Potpuna [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) objašnjava sigurno dostupnost Azure kao cjelinu.
 
### <a name="back-up-the-vm"></a>Stvaranje sigurnosne kopije na VM
 
[Oporavak servisa sigurnog](../backup/backup-introduction-to-azure-backup.md) se koristi za zaštitu podataka i resursima u servisa Azure sigurnosne kopije i oporavak Azure web-mjesta. Možete koristiti servise za oporavak sigurnog [implementacije](../backup/backup-azure-vms-automation.md)i upravljanja sigurnosne resursima implementiran VMs pomoću komponente PowerShell. 

## <a name="next-steps"></a>Daljnji koraci

- Ako je vaš cilj da biste radili s Linux VMs, pogledajte [Azure i Linux](virtual-machines-linux-azure-overview.md).
- Saznajte više o smjernice oko postavljanje preduvjete infrastrukture u [primjeru Azure infrastrukture vodič](virtual-machines-windows-infrastructure-example.md).
- Provjerite je li slijedite [Najbolje prakse za pokretanje sustava Windows VM na Azure](virtual-machines-windows-guidance-compute-single-vm.md).


