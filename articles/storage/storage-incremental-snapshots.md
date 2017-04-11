<properties
    pageTitle="Korištenje rastuće snimke za sigurnosno kopiranje i vraćanje Azure virtualnim strojevima | Microsoft Azure"
    description="Stvaranje prilagođenih rješenja za sigurnosno kopiranje i oporavak vaše diskova Azure virtualnog računala pomoću rastuća snimke."
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
    ms.date="10/18/2016"
    ms.author="aungoo"/>


# <a name="back-up-azure-virtual-machine-disks-with-incremental-snapshots"></a>Sigurnosno kopiranje diskova Azure virtualnog računala s rastuće snimke

## <a name="overview"></a>Pregled

Azure prostora za pohranu pruža mogućnost da biste preuzeli snimke blob-ova. Brze snimke snimanja blob stanja u tom trenutku u vremenu. U ovom se članku smo opisane scenarij od kako možete održavati sigurnosne kopije diskova virtualnog računala pomoću snimaka. U ovom methodology možete koristiti kad ne želite koristiti Azure sigurnosne kopije i servis za oporavak, a želite stvoriti prilagođene sigurnosne kopije Strategije za vaše diskova virtualnog računala.

Azure virtualnog računala diskova spremaju se kao BLOB-Ova stranica u odjeljku pohrana Azure. Jer smo razgovore o sigurnosne kopije strategija diskova virtualnog računala u ovom članku, ne možemo će upućuju na snimke u kontekstu blob-Ova stranica. Da biste saznali više o snimke, pogledajte [stvaranja brze snimke Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## <a name="what-is-a-snapshot"></a>Što je snimka?

Snimka blob je verzija blob koji se hvata u trenutku u vremenu koja je samo za čitanje. Nakon što snimku, ga možete biti čitati, kopirati, ili izbrisane, ali ne i mijenjati. Brze snimke omogućuje sigurnosno kopiranje blob kao što je prikazano u trenutku vremena. Do ostalih verzija 2015 04 05 imali mogućnost kopiranja cijelog snimke. S OSTATKOM verzija 2015-07-08 i iznad, koje možete i kopirati rastuće snimke.

## <a name="full-snapshot-copy"></a>Kopiranje cijelog snimke

Snimke se mogu kopirati na neki drugi račun za pohranu kao blob da biste zadržali sigurnosne kopije osnovne blob-om. Snimku možete kopirati i putem osnovni blob, čime vraćanje blob-om u starijoj verziji. Kada se snimka se kopiraju iz jednog računa za pohranu u drugu, će proširiti isti prostor kao blob osnovnu stranicu. Stoga Kopiranje cijele snimke s jednog računa za pohranu na drugi će biti sporo i i zauzeti mnogo prostora za pohranu računa ciljne.

>[AZURE.NOTE] Ako kopirate osnovni blob na drugo odredište, snimaka blob-om se ne kopiraju u njega. Isto tako, ako osnovni blob prebrisati kopiju, snimaka povezan s osnovnim blob ne utječe na i zadržavanje ostaje netaknuta pod nazivom osnovni blob.

### <a name="back-up-disks-using-snapshots"></a>Sigurnosno kopiranje diskova pomoću brze snimke

Kao sigurnosnu kopiju Strategije za vaše diskova virtualnog računala možete poduzeti periodičku snimke blob disk ili stranice i kopirajte ih na drugi račun za pohranu koji se pomoću alata kao što su [Kopiraj Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) operacija ili [AzCopy](storage-use-azcopy.md). Snimku možete kopirati blob stranice odredište pod drugim nazivom. Dobivene blob stranice odredište je blob snimanje stranice, a ne snimke. U nastavku ovog članka možemo će opisuju koraci koje morate proći sigurnosne kopije diskova virtualnog računala pomoću snimaka.

### <a name="restore-disks-using-snapshots"></a>Vraćanje diskova pomoću brze snimke

Kad dođe vrijeme da biste vratili na disku na prethodnu verziju stabilan zabilježene u jednom od sigurnosne kopije snimke, možete kopirati snimku putem blob osnovnu stranicu. Kada se snimka promovira osnovnu stranicu bloba ostane brze snimke, ali izvor prebrisat će kopiju koje se mogu i čitati i napisali. U nastavku ovog članka možemo opisane korake da biste vratili prethodnu verziju na disku iz njegova snimke.

### <a name="implementing-full-snapshot-copy"></a>Implementacijom kopiju cijelog snimke

Možete implementirati na sljedeće načine kopiju cijelog snimke

-   Prvo, stvorite snimku osnovni blob operacijom [Blob snimke](https://msdn.microsoft.com/library/azure/ee691971.aspx) .
-   Zatim kopirajte snimku računom prostora za pohranu za ciljnu pomoću [Blob Kopiraj](https://msdn.microsoft.com/library/azure/dd894037.aspx).
-   Ponovite ovaj postupak da biste zadržali sigurnosne kopije vaše osnovni blob.

## <a name="incremental-snapshot-copy"></a>Kopiraj rastuće snimke

Nova značajka u [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) API omogućuje mnogo bolje sigurnosno kopiranje snimke stranice blob-ova ili diskova. U API vraća popis svih promjena između osnovni blob i snimki. Time se smanjuje količinu prostora za pohranu koriste na sigurnosne kopije računa. U API podržava blob-Ova stranica na Premium prostora za pohranu, kao i standardne prostora za pohranu. Pomoću ovog API-JA, sada sastaviti brže i učinkovitije sigurnosne kopije rješenja za Azure VMs. To će biti dostupne od ostalih verzija 2015-07-08 i noviji.

Rastuća snimke Kopiraj omogućuje možete kopirati s jednog računa za pohranu na drugu razlikuju,

-   Osnovni blob i njegov snimka ili
-   Snimka dvije osnovne blob-om

Dobiveni su ispunjeni sljedeći uvjeti,

- Blob-om stvorena na Sij-1-2016 ili noviji.
- Blob-om neće prebrisati [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) ili [Kopiraj Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) između dva snimke.


**Napomena**: Ta je značajka dostupna za Premium i standardne Azure stranice blob-ova.

Kada imate prilagođene sigurnosne kopije strategije koji koristi snimke, kopiranje snimki s jednog računa za pohranu u drugu može biti vrlo sporo i troši mnogo prostora za pohranu. Umjesto da kopirate cijelu snimku s računom sigurnosne kopije prostora za pohranu, možete napisati razlika između uzastopna snimaka blob sigurnosne kopije stranice. Na taj način vremena za kopiranje i prostora za pohranu sigurnosne kopije značajno smanjiti.

### <a name="implementing-incremental-snapshot-copy"></a>Implementacijom Kopiraj rastuće snimke

Kopiraj rastuće snimke implementirati na sljedeće načine

-   Stvorite snimku osnovni blob pomoću [Blob snimke](https://msdn.microsoft.com/library/azure/ee691971.aspx).
-   Kopirali snimku račun sigurnosne kopije za pohranu za ciljnu pomoću [Blob Kopiraj](https://msdn.microsoft.com/library/azure/dd894037.aspx). To će biti blob sigurnosne kopije stranice. Stvorite snimku ovaj blob sigurnosne kopije stranice i spremanje sigurnosne kopije računa.
-   Iskoristite drugi snimku osnovni blob pomoću Blob snimke.
-   Pronađite razlika između prvi i drugi snimke osnovni blob pomoću [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx). Da biste odredili vrijednost datuma i vremena snimka želite izračunati razliku s pomoću nove parametar **prevsnapshot** . Kada je taj parametar izlaganje, OSTALE odgovor neće sadržavati samo stranice koje su se promijenile između cilj snimke i prethodne snimke uključujući Očisti stranice.
-   Pomoću [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) Primijeni te promjene na blob sigurnosne kopije stranice.
-   Na kraju, stvorite snimku blob sigurnosne kopije stranice i spremite ga na račun za sigurnosne kopije prostora za pohranu.

U sljedećem odjeljku, ne možemo opisane su detaljno kako možete održavati sigurnosne kopije diskova rastuće kopiji snimke

## <a name="scenario"></a>Scenarij

U ovom odjeljku ne možemo opisane su scenarija koji obuhvaća prilagođena sigurnosne kopije strategija diskova virtualnog računala pomoću snimaka.

Razmislite o VM za Azure DS niz s diskom P30 prostora za pohranu premium priložene. Disk P30 naziva *mypremiumdisk* pohranjuju se u račun za pohranu premium pod nazivom *mypremiumaccount*. Standardni prostora za pohranu račun pod nazivom *mybackupstdaccount* će se koristiti za spremanje sigurnosne kopije *mypremiumdisk*. Ne možemo želite zadržati snimku *mypremiumdisk* svaki 12 sati.

Dodatne informacije o stvaranju računa za pohranu i diskova, pogledajte [o Azure prostora za pohranu računa](storage-create-storage-account.md).

Da biste saznali više o sigurnosno kopiranje Azure VMs, pogledajte [Planiranje VM Azure sigurnosne kopije](../backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Korake da biste zadržali sigurnosne kopije na disku pomoću rastuće snimke

Koraci opisani niže će potrajati snimke *mypremiumdisk* i održavanje kopija u *mybackupstdaccount*. Stvaranje sigurnosne kopije bit će blob standardnoj stranici pod nazivom *mybackupstdpageblob*. Blob sigurnosne kopije stranice uvijek odražavaju istom stanju kao posljednje snimku *mypremiumdisk*.

1.  Prvo, stvorite sigurnosnu kopiju stranice blob za pohranu disk premium. Da biste to učinili, stvorite snimku *mypremiumdisk* naziva *mypremiumdisk_ss1*.
2.  Kopirajte ovu snimku mybackupstdaccount kao blob stranicu pod nazivom *mybackupstdpageblob*.
3.  Stvorite snimku *mybackupstdpageblob* naziva *mybackupstdpageblob_ss1*, pomoću [Blob snimku](https://msdn.microsoft.com/library/azure/ee691971.aspx) , a spremite ga na *mybackupstdaccount*.
4.  Tijekom prozoru sigurnosne kopije stvorite drugi snimku *mypremiumdisk*, izgovorite *mypremiumdisk_ss2*i spremite ga na *mypremiumaccount*.
5.  Pronađite rastuća promjena između dva snimke, *mypremiumdisk_ss2* i *mypremiumdisk_ss1*, pomoću [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) na *mypremiumdisk_ss2* **prevsnapshot** parametar postavljen na *mypremiumdisk_ss1*vremenske oznake. Sigurnosne kopije stranice blob *mybackupstdpageblob* u *mybackupstdaccount*za pisanje tih rastuća promjena. Ako su rastuća promjena izbrisane raspona, morate se očistiti iz sigurnosne kopije stranice blob-om. Pomoću [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) pisati rastuća promjena blob sigurnosne kopije stranice.
6.  Stvorite snimku u sigurnosne kopije stranice blob *mybackupstdpageblob*, pod nazivom *mybackupstdpageblob_ss2*. Brisanje prethodne snimke *mypremiumdisk_ss1* od premium prostora za pohranu računa.
7.  Ponovite korake 4-6 svakog sigurnosne kopije prozora. Na taj način možete održavati sigurnosne kopije *mypremiumdisk* u standardni prostora za pohranu računa.

![Stvaranje sigurnosne kopije na disku pomoću rastuće snimke](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Korake da biste vratili na disku iz snimke

Koraci opisani niže će vratiti premium disk, od *mypremiumdisk* na starije Brza snimka račun sigurnosne kopije za pohranu *mybackupstdaccount*.

1.  Odredite točke u vrijeme koje želite da biste vratili premium disku. Recimo da je snimka *mybackupstdpageblob_ss2*, koji je pohranjen u račun za sigurnosne kopije za pohranu *mybackupstdaccount*.
2.  U mybackupstdaccount, Promicanje snimke *mybackupstdpageblob_ss2* kao novi blob sigurnosne kopije osnovne stranice *mybackupstdpageblobrestored*.
3.  Stvorite snimku ovaj blob vraćene sigurnosne kopije stranice, pod nazivom *mybackupstdpageblobrestored_ss1*.
4.  Kopirajte blob vraćene stranice *mybackupstdpageblobrestored* iz *mybackupstdaccount* i *mypremiumaccount* kao novi disk premium *mypremiumdiskrestored*.
5.  Stvorite snimku *mypremiumdiskrestored*, poziva *mypremiumdiskrestored_ss1* buduće rastuće sigurnosne kopije.
6.  Točka DS niz VM da biste u vraćenom *mypremiumdiskrestored* na disku i odvajanje stari *mypremiumdisk* iz na VM.
7.  Započnite postupak sigurnosnog kopiranja opisane u prethodnom odjeljku za u vraćenom disk *mypremiumdiskrestored*, koristeći *mybackupstdpageblobrestored* kao blob sigurnosne kopije stranice.

![Vraćanje na disku sa snimkama](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o stvaranju snimke blob i planiranje preduvjete infrastrukture VM sigurnosnog kopiranja pomoću veze u nastavku.

- [Stvaranje snimke Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx)
- [Planiranje preduvjete VM infrastrukture za sigurnosne kopije](../backup/backup-azure-vms-introduction.md)
