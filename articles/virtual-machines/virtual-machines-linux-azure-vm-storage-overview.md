<properties
  pageTitle="Azure i pohranu Linux VM | Microsoft Azure"
  description="Opisuje Azure standardne i pohranu Premium s virtualnim strojevima Linux."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines-linux"
  authors="vlivech"
  manager="timlt"
  editor=""/>

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="NA"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure"
  ms.date="10/04/2016"
  ms.author="v-livech"/>

# <a name="azure-and-linux-vm-storage"></a>Azure i Linux VM prostora za pohranu

Azure prostora za pohranu je rješenje za pohranu oblaka za Moderna aplikacije koje se temelje na rok trajanja, dostupnost i skalabilnost potrebama svoje kupce.  Osim što za razvojne inženjere za izgradnju veliki aplikacija podržava nove scenarije, Azure prostora za pohranu omogućuje pohranu foundation virtualnim računalima sustava za Azure.

## <a name="azure-storage-standard-and-premium"></a>Azure prostor za pohranu: Standardno i Premium

Azure VM možete biti ugrađeni nakon standardne prostora za pohranu diskova ili diskova premium prostora za pohranu.  Kada pomoću portala za odabir vaše VM morate kada se prebacite na padajućem popisu na zaslonu osnove da biste pogledali standardne i premium diskova.  Snimka zaslona koja se nalazi ispod ističe te Uključivanje i isključivanje izbornika.  Kada na SSD, samo premium prostora za pohranu omogućeno VMs će se prikazati sve sigurnosno po SSD pogone.  Kada preklapa podizanje tvrdog diska, standardne prostora za pohranu omogućen VMs diskovnih pogona sigurnosnu kopiju adrese e će se prikazati uz premium prostora za pohranu VMs sigurnosno po SSD.

  ![screen1](../virtual-machines/media/virtual-machines-linux-azure-vm-storage-overview/screen1.png)

Prilikom stvaranja VM iz na `azure-cli` na raspolaganju standardnu i premium kada odaberete željenu veličinu VM putem na `-z` ili `--vm-size` eža zastavice.

### <a name="create-a-vm-with-standard-storage-vm-on-the-cli"></a>Stvaranje na VM pomoću standardnih spremište VM na na eža

Zastavica eža `-z` odabire Standard_A1 s A1 se standardni prostora za pohranu koji se temelje Linux VM.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_A1
```

### <a name="create-a-vm-with-premium-storage-on-the-cli"></a>Stvaranje na VM s premium prostora za pohranu na na eža

Zastavica eža `-z` odabire Standard_DS1 s DS1 temelji se premium prostora za pohranu Linux VM.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_DS1
```

## <a name="standard-storage"></a>Standardni prostora za pohranu

Azure standardne prostora za pohranu je Zadana vrsta prostora za pohranu.  Standardni prostora za pohranu stupa troškova dok još uvijek performant.  

## <a name="premium-storage"></a>Prostor za pohranu Premium

Prostor za pohranu Azure Premium nudi visokih performansi, najniža Latencija disk podrška za virtualnim strojevima radi li/O-ćete morati usko radnih opterećenja. Virtualni stroj (VM) diskova koji koristite za pohranu Premium spremiti podatke na pogonima pune stanje (SSDs). Možete migrirati diskova VM vaše aplikacije u spremište Premium Azure da iskoristite prednost brzine i performansi te diskova.

Značajke za pohranu Premium:

- Diskova za pohranu Premium: Azure Premium prostora za pohranu podržava VM diskova koje možete priložiti DS, DSv2 ili Oznaka nizova Azure VMs.

- Blob stranice Premium: Premium prostora za pohranu podržava Azure stranice blob-ova, koja se koriste za držite stalni diskova za Azure virtualnim strojevima (VMs).

- Lokalno suvišnih prostor za pohranu Premium: Račun za pohranu Premium samo podržava lokalno suvišnih prostora za pohranu (LRS) kao mogućnost za replikaciju i zadržava tri kopije podataka unutar jedno područje.

- [Prostor za pohranu Premium](../storage/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Prostor za pohranu Premium podržani VMs

Prostor za pohranu Premium podržava DS-serije, DSv2-, Oznaka niza i Fs niz Azure virtualnim strojevima (VMs). Standardna i Premium diskova za pohranu možete koristiti s Premium prostora za pohranu podržani od VMs. Ali ne možete koristiti za pohranu Premium diskova s VM niz koji nisu prostora za pohranu Premium kompatibilne.

Slijede distribucija Linux koje ćemo nastavlja s Premium prostora za pohranu.

| Distribuciju. | Verzija                 | Podržani otklanjanje    |
|--------------|-------------------------|---------------------|
| Ubuntu       | 12.04                   | 3.2.0-75.110+       |
| Ubuntu       | 14.04                   | 3.13.0-44.73+       |
| Debian       | 7.x, 8.x                | 3.16.7-ckt4-1+      |
| SLES         | SLES 12                 | 3.12.36-38.1+       |
| SLES         | SLES 11 SP4             | 3.0.101-0.63.1+     |
| CoreOS       | 584.0.0+                | 3.18.4+             |
| Centos       | 6.5, 6.6, 6,7, 7.0, 7.1 | 3.10.0-229.1.2.el7+ |
| RHEL         | 6.8 +, 7,2 +              |                     |


## <a name="file-storage"></a>Pohrani

Azure pohrani nudi zajedničko korištenje datoteka u oblak pomoću standardnih SMB protokol. Radite s datotekama Azure, možete migrirati enterprise aplikacije koje se temelje na poslužiteljima datoteka za Azure. Aplikacije koje rade u Azure možete jednostavno dostupnosti zajedničke datoteke iz Azure virtualnim strojevima izvodi Linux. A s najnovije izdanje prostora za pohranu datoteka, možete i postavljanje zajedničko korištenje datoteka iz lokalnog aplikacije koja podržava SMB 3.0.  Budući da zajedničke datoteke SMB dionice, možete im pristupiti putem standardni datotečni sustav API-ji.

Pohrani se temelji na istoj tehnologiji kao spremište blobova platforme, tablice i reda čekanja da pohrani nudi dostupnost, rok trajanja, skalabilnost i zemlj zalihosti koja je ugrađena u platforme Azure prostora za pohranu. Detalje o ciljeve za pohranu datoteka i ograničenja potražite u članku skalabilnost Azure prostora za pohranu i ciljeve.

- [Kako koristiti za pohranu datoteka Azure s Linux](../storage/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Tipkovni prostora za pohranu

Azure tipkovni prostora za pohranu sloju optimiziran je za pohranu podataka koji se često pristupa.  Prostor za pohranu tipkovni je Zadana vrsta prostora za pohranu za spremišta blobova platforme.

## <a name="cool-storage"></a>Odlične za pohranu

Azure odlične za pohranu sloju optimiziran je za pohranu podataka koji je diskovni pristupa i long-lived. Primjer koristi slučajeva za pohranu zanimljivih obuhvaćaju sigurnosne kopije, medijskih sadržaja, znanstvene podatke, sukladnost i arhiviranje podataka. Općenito govoreći, sve podatke koje se rijetko pristupa je savršen prijedlog za odlične za pohranu.

|                             | Razina tipkovni prostora za pohranu      | Razina odlične za pohranu     |
|:----------------------------|:---------------------:|:---------------------:|
| Dostupnost                | 99.9%                 | 99%                   |
| Dostupnost (RA GRS čitanja) | 99,99%                | 99.9%                 |
| Naknade za korištenje               | Veće troškove prostora za pohranu  | Manji trošak prostora za pohranu   |
|                             | LOWER programa access          | Veća programa access         |
|                             | i troškove transakcije | i troškove transakcije |


## <a name="redundancy"></a>Zalihosti

Podatke na računu sustava Microsoft Azure prostora za pohranu uvijek replicirati da bi se rok trajanja i visoke dostupnosti SLA prostora za pohranu Azure čak i in the face of tranzitne hardverske pogreške za sastanak.

Kada stvorite račun za pohranu, morate odabrati jednu od sljedećih mogućnosti replikacije:

- Lokalno suvišnih prostora za pohranu (LRS)
- Zone suvišnih prostora za pohranu (ZRS)
- Zemlj suvišnih prostora za pohranu (GRS)
- Pristup za čitanje zemlj suvišnih prostora za pohranu (RA GRS)

### <a name="locally-redundant-storage"></a>Lokalno suvišnih prostora za pohranu

Lokalno suvišnih prostora za pohranu (LRS) replicira podataka unutar područja u kojem ste stvorili račun za pohranu. Da biste maksimizirali rok trajanja, svaki zahtjev u odnosu na podatke na računu za pohranu je replicirati triput. Ove tri replike svaki se nalaze u zasebnom kvara domene i nadogradnje domene.  Zahtjev za uspješno vraća samo kada zapisan sve tri replike.

### <a name="zone-redundant-storage"></a>Zone suvišnih prostora za pohranu

Zone suvišnih prostora za pohranu (ZRS) replicira podataka preko dvije do tri funkcije unutar jedno područje ili preko dva područja koja omogućuje veći rok trajanja od LRS. Ako je vaš račun za pohranu ZRS omogućena, pa se podaci nalaze durable čak i u slučaju pogreške na jedan od funkcije.

### <a name="geo-redundant-storage"></a>Prostor za pohranu suvišnih zemlj.

Zemlj suvišnih prostora za pohranu (GRS) replicira podataka sekundarne područje koji je stotine milja izvan primarni regija. Ako je vaš račun za pohranu GRS omogućena, pa se podaci nalaze durable čak i ako potpuni regionalne prekida ili Izrada u kojem primarni regija nije koje se mogu vratiti.

### <a name="read-access-geo-redundant-storage"></a>Prostor za pohranu pristup za čitanje suvišnih zemlj.

Pristup za čitanje zemlj suvišnih prostora za pohranu (RA GRS) Maksimizira dostupnosti za vaš račun za pohranu unosom pristup podacima sekundarne mjestu, osim replikacije preko dva područja koje ste dobili od GRS samo za čitanje. U slučaju da podaci postaju dostupne u primarni regiji, aplikacija čita podatke s sekundarne regija.

Za precizno dive u Azure prostora za pohranu potražite u članku zalihosti:

- [Azure replikacije prostora za pohranu](../storage/storage-redundancy.md)

## <a name="scalability"></a>Skalabilnost

Azure prostora za pohranu je massively skalabilni, pa možete spremiti i proces stotine terabajta podataka koje podržava scenarije velikih skupova podataka potrebnih znanstvenom, financijske analize i multimedijskih aplikacija. Ili koji vam omogućuje pohranu manjih količina podatke potrebne za web-mjesto male tvrtke. Kad god se nalaziti vašim potrebama, plaćate samo za pohranjujete podatke. Azure prostora za pohranu trenutno sprema tens trillions jedinstveni korisnički objekata i u rukuje milijune za u sekundi.

Računa standardnu prostora za pohranu: račun za standardne prostora za pohranu ima najveći zahtjev za Ukupno rata 20 000 IOPS. Ukupna IOPS preko svih vaše diskova virtualnog računala u standardni prostora za pohranu računa smije to ograničenje.

Prostor za pohranu za račune za premium: račun za pohranu premium sadrži Maksimalna ukupna propusnost rata 50 Gbps. Ukupna propusnost preko svih vaše diskova VM smije to ograničenje.

## <a name="availability"></a>Dostupnost

Ne možemo jamči da barem 99,99% (99.9 za odlične pristup sloju) vremena, ne možemo obradit će se uspješno zahtjeve za čitanje podataka s računa za pohranu suvišnih pristup za čitanje – zemlj. (RA GRS), pod uvjetom da nije uspjelo pokušava pročitati podatke s područjem primarni ponoviti se na sekundarnom regija.

Ne možemo jamči da barem 99.9% (99% za odlične pristup sloju) vremena, ne možemo obradit će se uspješno zahtjeve za čitanje podataka iz lokalno suvišnih prostora za pohranu (LRS), Zone suvišnih prostora za pohranu (ZRS) i račune zemlj suvišnih prostora za pohranu (GRS).

Ne možemo jamči da barem 99.9% (99% za odlične pristup sloju) vremena, ne možemo uspješno obradit će zahtjevi za zapisivanje podataka lokalno suvišnih prostora za pohranu (LRS), Zone suvišnih prostora za pohranu (ZRS), i zemlj suvišnih prostora za pohranu (GRS) račune i račune za pohranu suvišnih pristup za čitanje – zemlj. (RA GRS).

- [Azure SLA za pohranu](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)


## <a name="regions"></a>Područja

Azure načelu dostupan je u 30 područja svijeta pa je objaviti tarife za 4 dodatne područja. Geografske proširenja je prioritet Azure jer omogućuje naših korisnika da biste postigli veću performansi te podržava njihove preduvjeti i preference vezane uz mjesto podataka.  Azures najnovije područja da biste pokrenuli se Njemačka.

Njemačka Microsoft Cloud nudi isporučujte prilagođena mogućnost za usluge Microsoft Cloud već dostupne u Europi, stvaranje povećana prilika za inovaciju i Ekonomske growth vrlo regulated partnerima i klijentima u Njemačka, Europske unije (EU) i Europske slobodno trgovini pridruživanje (EFTA).

Podatke o klijentu te nove u podatkovnim centrima u, u Magdeburg i Frankfurt, upravlja kontrolirati podataka povjerenički, Hrvatska sustavi za T, neovisno njemački tvrtke i podružnici Njemačke Telekom. Microsoftovim servisima komercijalni oblak u te podatkovnim centrima drži njemački podataka rukovanje propisa i klijentima pružili dodatni izbor gdje i kako se obrađuje podatke.


- [Mapiranje Azure regije](https://azure.microsoft.com/regions/)

## <a name="security"></a>Sigurnost

Azure prostora za pohranu nudi opsežan skup sigurnosne mogućnosti koji zajedno omogućiti inženjerima omogućuje stvaranje sigurnog aplikacije. Račun za pohranu sam mogu zaštiti pomoću na temelju uloga kontrola pristupa i Azure Active Directory. Podatke možete zaštićenim na putu između aplikacije i Azure pomoću klijentsko šifriranja, HTTPS ili SMB 3.0. Podatke možete postaviti automatski šifriranje kada zapisan Azure prostora za pohranu pomoću prostora za pohranu servisa šifriranja (SSE). OS i podataka koristi virtualnim strojevima diskova moguće je postaviti šifriranje pomoću šifriranja Azure Disk. Delegirana pristup objektima podataka u spremište Azure se mogu dodijeliti pomoću zajednički pristup potpisi.

### <a name="management-plane-security"></a>Upravljanje ravnini sigurnosti

Upravljanje ravnini sastoji se od resursa koji se koriste za upravljanje računa za pohranu. U ovom ćete odjeljku ćemo objasniti što model implementacije Voditelj resursa Azure te o tome kako na temelju uloga kontrole pristupa (RBAC) za kontrolu pristupa koristite za pohranu računi. Ne možemo će i razgovarati o upravljanju ključeva za račun za pohranu i kako ih Obnovi.

### <a name="data-plane-security"></a>Sigurnost ravnini podataka

U ovom ćete odjeljku smo ćete pogledajte dopuštanja pristupa objekata stvarnih podataka na vašem računu za pohranu, kao što su blob-ova, datoteke, redovi i tablice, pomoću zajednički pristup potpisa i spremljene pravilnike za pristup. Ne možemo obrađuje SAS razini usluge i SAS razinom računa. Ne možemo prikazat će se i kako se ograničava pristup određene IP adrese (ili raspon IP adresa), kako ograničiti protokol koji se koristi za HTTPS i kako opozvati pristup potpis zajedničko korištenje bez čekanja da bi isteći.

## <a name="encryption-in-transit"></a>Šifriranje na putu

U ovom se odjeljku opisuje kako zaštite podataka kada prenosite pojavljivanje ili iščezavanje Azure prostora za pohranu. Ćemo objasniti što preporučuje korištenje HTTPS i šifriranje koristi SMB 3.0 za Azure zajedničke datoteke. Ne možemo, će bacite pogled na klijentskoj strani šifriranja koja omogućuje šifriranje podataka prije nego što se prenosi u prostor za pohranu u klijentskoj aplikaciji i dešifrirati podatke nakon prenose se više mjesta za pohranu.

## <a name="encryption-at-rest"></a>Šifriranje na ostale

Će ćemo objasniti prostora za pohranu servisa šifriranja (SSE) te kako možete omogućiti je za račun za pohranu, rezultira blob polja blok, stranice blob-ova i dodavanje blob polja koja se automatski šifrirane kada zapisan Azure prostora za pohranu. Ne možemo će i pogledajte kako možete koristiti šifriranje Azure i Istražite osnovne razlike i slučajeva šifriranje nasuprot SSE nasuprot klijentsko šifriranje. Ne možemo će ukratko pogledajte FIPS usklađenost za računala vlada SAD-a.

- [Vodič za sigurnost Azure prostora za pohranu](../storage/storage-security-guide.md)

## <a name="cost-savings"></a>Računanje troškova

- [Prostor za pohranu trošak](https://azure.microsoft.com/pricing/details/storage/)

- [Kalkulator za pohranu trošak](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Ograničenja prostora za pohranu

- [Ograničenja prostora za pohranu servisa](../azure-subscription-service-limits.md#storage-limits)
