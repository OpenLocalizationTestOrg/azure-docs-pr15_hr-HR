<properties
    pageTitle="Smjernice za pohranu rješenja | Microsoft Azure"
    description="Saznajte više o ključa dizajna i implementaciju smjernice za implementaciju rješenja za pohranu servisa Azure infrastrukture."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="storage-infrastructure-guidelines"></a>Smjernice za infrastrukturu za pohranu

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

U ovom članku fokus je na razumijevanje potrebe za pohranu i pitanja vezana uz dizajna za postizanje performanse najbolju virtualnog računala (VM).


## <a name="implementation-guidelines-for-storage"></a>Smjernice za implementaciju za pohranu

Odluka:

- Morate koristiti standardni prikaz ili Premium prostora za pohranu za svoje radno opterećenje?
- Morate li na disku Podjela da biste stvorili diskova veća od 1023 GB?
- Morate li na disku Podjela da biste postigli optimalne performanse/i za svoje radno opterećenje?
- Što postavljanje računa za pohranu potrebno hostira radno opterećenje IT ili infrastrukture?

Zadaci:

- Pregledajte/i zahtjeve aplikacija implementacije i planiranje odgovarajući broj i vrsta računa za pohranu.
- Stvaranje skupa za pohranu računima koji se koriste na konvencija imenovanja. Možete koristiti EŽA Azure ili portalu.


## <a name="storage"></a>Prostor za pohranu

Azure prostora za pohranu je ključa dio implementacije i upravljanja virtualnim strojevima (VMs) i aplikacije. Azure prostora za pohranu omogućuje servise za pohranu podataka iz datoteke, nestrukturirane podatke i poruke, a ujedno dio infrastrukture VMs za podršku.

Za podršku VMs dostupne su dvije vrste računa za pohranu:

- Standardni prostora za pohranu računi izlaganje pristup sa spremištem blobova (koristi se za pohranu Azure VM diskova) spremište tablica, reda čekanja za pohranu i pohranu datoteka.
- Računi [za pohranu Premium](../storage/storage-premium-storage.md) isporučiti visokih performansi, najniža Latencija disk podrška za/i intenzivno radnih opterećenja, kao što su MongoDB Sharded klaster. Prostor za pohranu Premium trenutno podržava samo Azure VM diskova.

Azure stvara VMs s na disk operacijski sustav, privremeno na disku i nula ili više diskova neobavezne podatke. Operacijski sustav na disku i diskova podataka su blob-Ova stranica Azure, dok je privremeni disk spremiti lokalno na čvor gdje se nalaze na računalu. Pobrinuti se prilikom dizajniranja aplikacije koje će se koristiti samo privremene disk za podatke koji nisu stalni kao na VM može migrirati između domaćini tijekom održavanja događaja. Podatke pohranjene na disku za privremene će se izgubiti.

Rok trajanja i visoke dostupnosti osigurava podlozi okruženje za pohranu Azure da bi vaši podaci ostaju zaštićeni protiv neplanirano održavanja ili hardver pogreške. Dizajniranje okruženje za Azure prostora za pohranu, možete odabrati za replikaciju VM prostora za pohranu:

- lokalno unutar zadanog Azure podatkovnim centrom
- preko Azure podatkovnim centrima unutar zadanog područja
- preko Azure podatkovnim centrima preko različitih područja.

Saznajte [više o mogućnostima replikacije visoke dostupnosti](../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Operacijski sustav diskova i diskova podataka imaju maksimalnu veličinu 1023 gigabajta (GB). Maksimalna veličina blob je 1024 GB, a koji moraju sadržavati metapodataka (Podnožje) VHD datoteke (u GB je 1024<sup>3</sup> bajtova). Da biste surpass to ograničenje po okupljanje zajedno diskova podataka radi prikazivanja logičke količine veće od 1023GB vaše VM, poslužite se logički glasnoću Manager (LVM).

Postoje neka ograničenja skalabilnost prilikom dizajniranja implementacijama sustava Azure prostora za pohranu – potražite u članku [ograničenja pretplate i servisa Microsoft Azure, kvote, i ograničenjima](azure-subscription-service-limits.md#storage-limits) više pojedinosti. Pogledati i [Azure prostora za pohranu skalabilnost i performanse ciljnih web-mjesta](../storage/storage-scalability-targets.md).

Za aplikacije za pohranu, možete spremiti nestrukturirane objekt podataka kao što su dokumenti, slike, sigurnosne kopije, konfiguracijskih podataka, zapisnika itd. Korištenje spremišta blobova. Umjesto aplikacije pisanje virtualne disk priložiti u VM, aplikacija možete pisati izravno u spremište blobova platforme Azure. Spremište blobova platforme nudi mogućnost [Tipkovni i zanimljivih pohranu razine](../storage/storage-blob-storage-tiers.md) ovisno o dostupnosti potrebama i ograničenja troškova.


## <a name="striped-disks"></a>Prugastim diskova
Osim koja vam omogućuje stvaranje diskova veća od 1023 GB u više instanci pomoću Podjela diskova podataka poboljšava performanse dopuštanjem više blob-ova u pozadinu prostora za pohranu za jednu jedinicu. S Podjela, / i potrebne za pisanje i čitanje podataka iz jedne logičkog diska nastavlja se paralelno.

Azure nameće ograničenja na broj diskova podataka i količinu dostupne, ovisno o veličini VM propusnosti. Detalje potražite u članku [veličine za virtualnih računala](virtual-machines-linux-sizes.md).

Ako koristite Podjela na disku za diskova Azure podataka, preporučuje se sljedećih smjernica:

- Diskova podataka mora uvijek biti maksimalne veličine (1023 GB).
- Priloži diskova podataka maksimalne dopuštene veličine VM.
- Koristite LVM.
- Izbjegavanje Azure podatkovni disk predmemoriranje mogućnosti (predmemoriranje pravila = ništa).

Dodatne informacije potražite u članku [Konfiguriranje LVM na Linux VM](virtual-machines-linux-configure-lvm.md).


## <a name="multiple-storage-accounts"></a>Više prostora za pohranu računa

Prilikom dizajniranja okruženje za Azure prostora za pohranu, možete koristiti više prostora za pohranu računa kao broj VMs implementacije povećava. Taj se način olakšava izgleda u/i raspodijelite podlozi infrastruktura za pohranu Azure da biste zadržali optimalne performanse za VMs i aplikacije. Prilikom dizajniranja aplikacije koje su implementacija, razmislite o/i preduvjeti svaki VM sadrži i saldo izgleda te VMs u prostor za pohranu Azure računa. Pokušajte izbjeći grupiranje svih na visok/i zahtjevne VMs u samo jednom ili dvjema račune za pohranu.

Dodatne informacije o mogućnostima/i različite mogućnosti Azure prostora za pohranu i neke preporučujemo da maximums potražite [Azure prostora za pohranu skalabilnost i performanse ciljnih web-mjesta](../storage/storage-scalability-targets.md).


## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 