<properties
    pageTitle="O i VHDs za Linux VMs | Microsoft Azure"
    description="Saznajte više o osnovama diskova i VHDs za Linux virtualnim strojevima u Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>O i VHDs za Azure virtualnim strojevima

Baš kao i bilo kojeg drugog računala virtualnim strojevima u Azure koristite diskova kao mjesto za pohranu operacijski sustav, aplikacije i podataka. Sve Azure virtualnim računalima imati najmanje dva diskova – na disku operacijski sustav Linux (u slučaju Linux VM) i privremene disk. Operacijski sustav disku stvara se slike, a na disku operacijski sustav i slika su zapravo virtualne tvrdom disku (VHDs) na pohranjene u račun za Azure prostora za pohranu. Virtualnim strojevima može imati jedan ili više diskova podatke pohranjene kao VHDs. U ovom se članku i dostupna je za [virtualnim strojevima sa sustavom Windows](virtual-machines-windows-about-disks-vhds.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

> [AZURE.NOTE] Ako imate nekoliko sekundi dok, Pridonesite poboljšanju dokumentaciju Azure Linux VM prihvaćanjem ovaj [Brzi upitnik](https://aka.ms/linuxdocsurvey) rad. Svaki odgovor pridonose pomoć zatražite posla.

## <a name="operating-system-disk"></a>Operacijski sustav na disku

Svaki virtualnog računala sadrži jedan disk priloženu operacijski sustav. On je registrirana kao SATA pogon i koje su označene kao pogon C: po zadanom. Disk je maksimalna kapacitet 1023 gigabajta (GB). 

##<a name="temporary-disk"></a>Privremeni disk

Privremeni disk automatski stvara za vas. Na Linux virtualnim strojevima disk obično /dev/sdb koji je oblikovan i postavljen da /mnt/resource po Agent za Azure Linux.

Veličina privremene disk razlikuju, ovisno o veličini virtualnog računala. Dodatne informacije potražite u članku [veličine za virtualnim strojevima Linux](virtual-machines-linux-sizes.md).

>[AZURE.WARNING] Ne Pohranjujte podataka na privremeni disk. Služi kao privremeno spremište za aplikacije i procesa, a namijenjen samo pohranjujete podatke kao što su stranice ili Zamjena datoteke. 

Dodatne informacije o kako Azure koristi privremene disk, pročitajte [objašnjenje privremene pogon na virtualnim strojevima programa Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Podatkovni disk

Podatkovni disk je VHD koja je priložena virtualnog računala radi pohrane podataka aplikacije ili druge podatke morate zadržati. Diskova podataka registrirane kao SCSI pogona i označavaju slovom koje odaberete.  Svaki podatkovni disk kapaciteta Maksimalna 1023 GB. Veličina virtualnog računala određuje koliko podataka diskova možete priložiti i vrsta mjesta za pohranu možete koristiti za hostiranje na diskova.

>[AZURE.NOTE] Dodatne pojedinosti o kapaciteta virtualnim računalima potražite u članku [veličine za virtualnim strojevima Linux](virtual-machines-linux-sizes.md).

Azure stvara na disk operacijski sustav prilikom stvaranja virtualnog računala slike. Ako koristite sliku koja sadrži podatke diskova, Azure i stvara diskova podataka kada se stvara virtualnog računala. U suprotnom, dodajte diskova podataka nakon što stvorite virtualnog računala.

Možete dodati podatke diskova virtualnog računala u bilo kojem trenutku prema **priložite** disk da biste virtualnog računala. Možete koristiti VHD koji ste prenijeli ili kopirali računa za pohranu ili onu koja je za vas stvara Azure. Prilaganje podatkovni disk pridružuje VHD datoteku s računa za pohranu virtualnog računala potvrđivanjem "Zakup" na na VHD tako da se ne može izbrisati iz spremišta dok je i dalje pridružen.

## <a name="about-vhds"></a>O VHDs

VHDs koji se koriste u Azure su .vhd datoteke spremljene kao BLOB-Ova stranica na računu za pohranu standardno ili premium u Azure. Detalje o blob-Ova stranica, potražite u članku [objašnjenje bloka blob-ova i blob-Ova stranica](https://msdn.microsoft.com/library/ee691964.aspx). Detalje o premium za pohranu potražite u članku [prostora za pohranu Premium: visokih performansi prostor za pohranu radnih opterećenja Azure virtualnog računala](../storage/storage-premium-storage.md).

Azure podržava format VHD fiksni disk. Nepromjenjivi oblik lays logičkog diska Linearno unutar datoteke, tako da se taj disk Pomak X pohranjen na blob Pomak X. Mali podnožja na kraju blob-om opisuju svojstva na VHD. Često nepromjenjivi oblik troši prostor jer je Većina diskova velike Neiskorišteni raspone u njima. Međutim, Azure sprema .vhd datoteke u obliku kratke tako primaju prednosti na fiksni i dinamički diskova u isto vrijeme. Dodatne informacije potražite u članku [Uvod u rad s virtualne tvrdi disk](https://technet.microsoft.com/library/dd979539.aspx).

Sve datoteke .vhd u Azure koji želite koristiti kao izvor da biste stvorili diskova ili slike su samo za čitanje. Kada stvorite disk ili sliku, Azure čini kopije datoteka .vhd. Te kopije može biti samo za čitanje ili i-čitanje, ovisno o tome kako koristiti u VHD.

Kada stvorite virtualnog računala slike, Azure stvara na disku za virtualnog računala koji je kopija iz izvorišne datoteke .vhd. Da biste zaštitili nenamjerno brisanje, Azure smješta u Zakup na bilo koju datoteku .vhd izvora koji se koristi za stvaranje slike, na disk operacijski sustav ili podatkovni disk.

Prije nego što možete izbrisati izvorne datoteke .vhd, morat ćete ukloniti u Zakup brisanjem disk ili slike. Da biste izbrisali datoteku .vhd koja koristi virtualnog računala kao na disk operacijski sustav, možete izbrisati virtualnog računala na disku operacijski sustav i izvornu datoteku .vhd odjednom tako da izbrišete virtualnog računala i izbrišete sve povezane diskova. Međutim, brisanje .vhd datoteku koja je izvor za podatkovni disk potrebne nekoliko koraka redoslijedom postavljanje--odvajanje disk s virtualnog računala, izbrišite disk, a zatim izbrišite .vhd datoteku.

>[AZURE.WARNING] Ako brisanje izvorne datoteke za .vhd sa servisa za pohranu ili brisanje računa za pohranu, Microsoft nije moguće vratiti podatke umjesto vas.


## <a name="troubleshooting"></a>Otklanjanje poteškoća
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Daljnji koraci

-  [Priloži na disku](virtual-machines-linux-add-disk.md) da biste dodali dodatan prostor za pohranu za vaše VM.
-  [Konfiguriranje softver RAID](virtual-machines-linux-configure-raid.md) za zalihosti.
-  Da bi se brzo možete implementirati dodatne VMs, [snimiti Linux virtualnog računala](virtual-machines-linux-classic-capture-image.md) .


