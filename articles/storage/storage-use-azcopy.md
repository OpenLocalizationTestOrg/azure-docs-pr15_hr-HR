<properties
    pageTitle="Kopiranje i premještanje podataka za pohranu s AzCopy | Microsoft Azure"
    description="Koristite AzCopy utility za premještanje i kopiranje podataka ili s blob, tablice i sadržaja datoteke. Kopirajte podatke u spremište Azure iz lokalne datoteke ili kopiranje podataka unutar ili između računa za pohranu. Jednostavno premještanje podataka za pohranu Azure."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="micurd"/>

# <a name="transfer-data-with-the-azcopy-command-line-utility"></a>Prijenos podataka s AzCopy naredbenog retka uslužni

## <a name="overview"></a>Pregled

AzCopy je Windows naredbenog retka utility namijenjen kopiranje podataka da biste i iz spremišta blobova platforme Microsoft Azure, datoteke i tablici pomoću jednostavne naredbe optimalne performanse. Podatke možete kopirati iz jednog objekta na drugu unutar vašeg računa za pohranu ili između računa za pohranu.

> [AZURE.NOTE] Ovaj vodič pretpostavlja da ste već upoznati s [Azure prostora za pohranu](https://azure.microsoft.com/services/storage/). Ako nije, u dokumentaciji [Uvod u Azure prostora za pohranu](storage-introduction.md) za čitanje će biti koristan. Većina važno je napomenuti da, morat ćete [stvoriti račun za pohranu](storage-create-storage-account.md#create-a-storage-account) da biste počeli koristiti AzCopy.

## <a name="download-and-install-azcopy"></a>Preuzmite i instalirajte AzCopy

### <a name="windows"></a>Windows

Preuzmite [najnoviju verziju AzCopy](http://aka.ms/downloadazcopy).

### <a name="maclinux"></a>Mac i Linux

AzCopy nije dostupna za Mac i Linux OSs. Međutim, Azure EŽA je odgovarajuću alternative za kopiranje podataka i iz Azure prostora za pohranu. Pročitajte [pomoću EŽA Azure s Azure pohranom](storage-azure-cli.md) da biste saznali više.

## <a name="writing-your-first-azcopy-command"></a>Prvi AzCopy naredbu za pisanje

Osnovni Sintaksa naredbe AzCopy je:

    AzCopy /Source:<source> /Dest:<destination> [Options]

Otvorite naredbeni prozor, a zatim otvorite direktoriju AzCopy instalacije na računalu – mjesto na `AzCopy.exe` izvršna nalazi. Po želji možete dodati mjesto instalacije AzCopy na putu sustava. Prema zadanim postavkama AzCopy instaliran `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` ili `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

Sljedeći primjeri prikazuju raznih scenariji za i kopiranje podataka iz Microsoft Azure blob-ova, datoteke i tablice. Pogledajte odjeljak [AzCopy parametara](#azcopy-parameters) za Detaljno objašnjenje parametara koji se koriste u svaki uzorak.

## <a name="blob-download"></a>Blob: preuzimanje

### <a name="download-single-blob"></a>Preuzimanje jedan blobova platforme

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"

Primijetite da, ako u mapu `C:\myfolder` ne postoji, AzCopy će ga stvoriti i preuzimanje `abc.txt ` u novu mapu.

### <a name="download-single-blob-from-secondary-region"></a>Preuzmite jedan blob iz sekundarne regija

    AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Imajte na umu da morate imati pristup za čitanje zemlj suvišne za pohranu omogućena.

### <a name="download-all-blobs"></a>Preuzimanje svih blob-ova

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

Pretpostavimo da sljedeće blob-ova koji se nalaze u navedenom spremnik:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Nakon preuzimanja operacije direktoriju `C:\myfolder` neće sadržavati sljedeće datoteke:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Ako ne navedete mogućnost `/S`, bez blob-Ova će se preuzeti.

### <a name="download-blobs-with-specified-prefix"></a>Preuzimanje blob polja s navedenom prefiksa

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

Pretpostavimo da sljedeće blob-ova koji se nalaze u navedenom spremnik. Sve blob-ova počevši s prefiksom `a` će se preuzeti:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Nakon preuzimanja operacije, mapu `C:\myfolder` neće sadržavati sljedeće datoteke:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Prefiks primjenjuje se na virtualnog direktorija koji prvi dio naziva blobova platforme za obrasce. U primjeru iznad, virtualnog direktorija ne odgovara navedeni prefiks tako da nije preuzet. Osim toga, ako je mogućnost `\S` nije naveden, AzCopy će preuzeti sve blob-ova.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Postavljanje vremena posljednji izmijenio izvezene datoteka koje će biti isti kao izvor blob-ova

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

Možete isključiti i blob-ova iz preuzimanje operacija na temelju njihove posljednji izmijenio vremena. Ako, na primjer, ako želite izostaviti blob-ova čiji je posljednji izmijenio vrijeme je iste ili novija od odredišnu datoteku dodati u `/XN` mogućnost:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

Ili ako želite izostaviti blob-ova čiji je posljednji izmijenio vrijeme je isti ili starije od odredišnu datoteku dodate u `/XO` mogućnost:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

## <a name="blob-upload"></a>Blob: prijenos

### <a name="upload-single-file"></a>Prijenos jedne datoteke

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"

Ako ne postoji spremnik navedeno odredište, AzCopy će ga stvoriti i prenijeti datoteku na nju.

### <a name="upload-single-file-to-virtual-directory"></a>Prijenos jedne datoteke virtualnog direktorija

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

Ako ne postoji navedeni virtualni direktorij, AzCopy prenijeti datoteku da biste uvrstili virtualnog direktorija u nazivu (*npr.*, `vd/abc.txt` u gornjem primjeru).

### <a name="upload-all-files"></a>Prijenos svih datoteka

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

Određivanje mogućnosti `/S` prenosi sadržaj navedeni direktorij rekurzivno spremište blobova platforme, što znači da sve podmape i datoteke će biti prenesene kao i. Na primjer, pretpostavimo da sljedeće datoteke nalaze u mapi `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Nakon operacije prijenos spremnik neće sadržavati sljedeće datoteke:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Ako ne navedete mogućnost `/S`, AzCopy će prijenos rekurzivno. Nakon operacije prijenos spremnik neće sadržavati sljedeće datoteke:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Prijenos datoteka koje odgovaraju navedenim uzorkom

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

Pretpostavimo da sljedeće datoteke nalaze u mapi `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Nakon operacije prijenos spremnik neće sadržavati sljedeće datoteke:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Ako ne navedete mogućnost `/S`, AzCopy samo prijenos blob polja koje se ne nalaze u virtualni imenik:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Odredite MIME vrsta sadržaja blob odredište

Prema zadanim postavkama AzCopy postavlja vrstu sadržaja blob odredište da biste `application/octet-stream`. Počevši od verzije 3.1.0, možete izričito odrediti vrstu sadržaja putem mogućnosti `/SetContentType:[content-type]`. Ova sintaksa postavlja vrste sadržaja za sve blob-ova u operaciji prijenos.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

Ako navedete `/SetContentType` bez vrijednosti, zatim AzCopy postavit svaki blob ili vrstu sadržaja datoteke prema njezin datotečni nastavak.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## <a name="blob-copy"></a>Blob: kopiranje

### <a name="copy-single-blob-within-storage-account"></a>Kopirajte jedan blob subjekta prostora za pohranu

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt

Kada kopirate blob subjekta za pohranu, izvodi se na [strani poslužitelja](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) kopiranja.

### <a name="copy-single-blob-across-storage-accounts"></a>Kopirajte jedan blob preko račune za pohranu

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Kada kopirate blob preko račune za pohranu, provodi operacija [kopiranja poslužiteljsko](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) .

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Kopiranje jedan blob iz sekundarne regija u primarni regija

    AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Imajte na umu da morate imati pristup za čitanje zemlj suvišnih za pohranu omogućena.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Kopirajte jedan blob i njegov snimke u prostor za pohranu računa

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

Nakon postupak kopiranja spremnik cilj neće sadržavati stavku blob-om i njegov snimke. Pod pretpostavkom da blob u gornjem primjeru sadrži dvije snimke, spremnik neće sadržavati sljedeće blob i snimaka:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Sinkrono kopirajte blob-ova u prostor za pohranu računa

AzCopy po zadanom asinkrono kopira podataka između dva krajnje točke za pohranu. Stoga kopiranja funkcionirat će u pozadini pomoću kapaciteta propusnosti rezervnih koja ima bez SLA pomoću kako brzo blob kopirat će se i AzCopy će povremeno status Kopiraj dok kopiranja ne dovrši ili nije uspjelo.

Na `/SyncCopy` mogućnost osigurava kopiranja će dobiti dosljedan brzinu. AzCopy izvodi sinkronizirano Kopiraj preuzimanjem blob polja da biste kopirali navedeni izvor lokalne memorije i zatim ih prenesete na odredište spremište blobova platforme.

    AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

`/SyncCopy`možda generiranje dodatne izlazne trošak u usporedbi s asinkronog Kopiraj, preporučeni način je da biste koristili tu mogućnost u programa Azure VM koji se nalazi u istom području kao račun za pohranu izvor da bi se izbjeglo izlazne trošak.

## <a name="file-download"></a>Datoteka: preuzimanje

### <a name="download-single-file"></a>Preuzimanje samo jedne datoteke

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Ako je navedena izvor Azure zajedničkom, a zatim navedite naziv točno datoteke (*npr.* `abc.txt`) za preuzimanje u jednoj datoteci ili odredite mogućnost `/S` da biste preuzeli sve datoteke u rekurzivno zajedničko korištenje. Pokušaj da biste odredili uzorak datoteke i mogućnosti `/S` zajedno uzrokovat će pogrešku.

### <a name="download-all-files"></a>Preuzimanje svih datoteka

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S

Imajte na umu da sve mape za prazan će se preuzeti.

## <a name="file-upload"></a>Datoteka: prijenos

### <a name="upload-single-file"></a>Prijenos jedne datoteke

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt

### <a name="upload-all-files"></a>Prijenos svih datoteka

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

Imajte na umu da sve mape za prazan se neće prenijeti.

### <a name="upload-files-matching-specified-pattern"></a>Prijenos datoteka koje odgovaraju navedenim uzorkom

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

## <a name="file-copy"></a>Datoteka: kopiranje

### <a name="copy-across-file-shares"></a>Kopiranje preko zajedničke datoteke

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

### <a name="copy-from-file-share-to-blob"></a>Kopirajte iz zajedničko korištenje datoteka i bloba

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

Imajte na umu da asinkronog kopiranje s pohrani Blob stranice nije podržana.

### <a name="copy-from-blob-to-file-share"></a>Kopiranje blobova platforme za zajedničko korištenje datoteka

    AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

### <a name="synchronously-copy-files"></a>Sinkrono kopiranje datoteka

Možete odrediti na `/SyncCopy` mogućnosti da biste kopirali podatke sa servisa za pohranu datoteka za pohranu datoteka, iz datoteke spremište blobova i spremište blobova platforme za pohranu datoteka sinkrono, AzCopy to tako da preuzmete izvorišnih podataka u lokalnom memoriju i prenesite ga ponovno odredište.

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

Prilikom kopiranja iz pohrani sa spremištem blobova, zadana vrsta blob je blob blok, korisnik može odrediti mogućnost `/BlobType:page` da biste promijenili odredišnu vrstu blob.

Imajte na umu da `/SyncCopy` možda generiranje dodatne izlazne cijena Usporedba asinkronog kopiju, preporučeni način je da biste koristili tu mogućnost u VM Azure koje se nalazi u istom području kao račun za pohranu izvor da bi se izbjeglo izlazne trošak.

## <a name="table-export"></a>Tablica: izvoz

### <a name="export-table"></a>Izvoz tablice

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

AzCopy piše manifesta datoteku navedenu odredišnu mapu. Datoteka manifesta se koristi u uvoza da biste pronašli potrebne podatkovne datoteke i provjeru valjanosti podataka. Datoteka manifesta po zadanom koristi konvencije imenovanja sljedeće:

    <account name>_<table name>_<timestamp>.manifest

Korisnik može odrediti mogućnost `/Manifest:<manifest file name>` da biste postavili naziv manifesta datoteke.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest

### <a name="split-export-into-multiple-files"></a>Podjela Izvoz u više datoteka

    AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

AzCopy koristi *glasnoću indeksa* u nazivima datoteka podjelu podataka za razlikovanje više datoteka. Indeks jedinica sastoji se od dva dijela *particija ključa raspon indeksa* i *Podjela indeks datoteka*. Polje su oba indeksi.

Particija indeksa ključa raspona će biti 0 Ako korisnik odrediti mogućnost `/PKRS`.

Na primjer, pretpostavimo da AzCopy generira dvije podatkovne datoteke kada korisnik navede mogućnost `/SplitSize`. Dobiveni nazivi datoteka podataka može biti:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Imajte na umu da vrijednost mogućnost minimalnu moguće `/SplitSize` 32 MB. Ako je navedeno odredište blobova, AzCopy će Podjela podatkovnu datoteku nakon njegove veličine dosegne ograničenje veličine blob (200GB), bez obzira je li mogućnost `/SplitSize` određen korisnik.

### <a name="export-table-to-json-or-csv-data-file-format"></a>Izvoz tablice JSON ili CSV oblik datoteke podataka

AzCopy po zadanom izvoz tablica u JSON podatkovne datoteke. Možete navesti mogućnost `/PayloadFormat:JSON|CSV` za izvoz tablica u obliku JSON ili CSV.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

Prilikom određivanja tereta oblik CSV AzCopy i generiranje sheme datoteku s nastavkom `.schema.csv` za svaku podatkovnu datoteku.

### <a name="export-table-entities-concurrently"></a>Izvoz tablice entiteti istovremeno

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

AzCopy će se pokrenuti istovremene operacije izvoza entiteti kada korisnik navede mogućnost `/PKRS`. Svaka operacija izvozi jedan raspon ključa particije.

Imajte na umu da broj istovremene operacije i upravljaju mogućnost `/NC`. AzCopy koristi broja procesora koji core kao zadanu vrijednost od `/NC` pri kopiranju entiteti tablice, čak i ako `/NC` nije naveden. Kada korisnik navede mogućnost `/PKRS`, AzCopy koristi manje od te dvije vrijednosti – particija ključa raspona nasuprot neizravno ili izravno koja je navedena istovremene operacije - odrediti broj istovremene operacije da biste pokrenuli. Dodatne informacije, upišite `AzCopy /?:NC` u naredbenom retku.

### <a name="export-table-to-blob"></a>Izvoz tablice da biste bloba

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

AzCopy generirat će JSON podatkovne datoteke u kontejner s blob s pratiti konvencija imenovanja:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Generirana podatkovne datoteke u JSON slijedi oblik tereta minimalnog metapodataka. Detalje o tom obliku tereta potražite u članku [Oblikovanje teret za operacije servisa tablice](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Imajte na umu da kada se izvoze tablice da biste blob-ova, AzCopy će preuzmite entiteti tablice na lokalni privremene podatkovne datoteke i zatim prenesite te entiteti blob-om. Ove privremene podatkovne datoteke spremaju se u mapu dnevnik putom zadani "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", možete odrediti mogućnost/Z: [dnevnik--mapa] da biste promijenili dnevnik datoteke mjesto mape i stoga promijenili mjesto za privremene podatkovne datoteke. Veličina privremene podatkovne datoteke navedena je veličina entiteti na tablice i veličine koje ste naveli s /SplitSize mogućnost, iako privremene podatkovne datoteke u lokalnom disku izbrisat će se odmah nakon preneseni blob-om, provjerite imate li dovoljno prostora na lokalnom disku za pohranu ove privremene podatkovne datoteke, prije brisanja.

## <a name="table-import"></a>Tablica: Uvoz

### <a name="import-table"></a>Uvoz tablice

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace

Mogućnost `/EntityOperation` pokazuje kako umetnuti entiteti u tablici. Moguće vrijednosti su:

- `InsertOrSkip`: Preskače postojeći entitet ili umeće novi entitet, ako ne postoji u tablici.
- `InsertOrMerge`: Spaja postojeći entitet ili umeće novi entitet, ako ne postoji u tablici.
- `InsertOrReplace`: Zamjenjuje postojeći entitet ili umeće novi entitet, ako ne postoji u tablici.

Napomena ne možete navesti mogućnost `/PKRS` u scenariju uvoz. Za razliku od scenarij izvoz, u kojem morate navesti mogućnost `/PKRS` da biste započeli istovremene operacije, AzCopy po zadanom počet će istovremene operacije prilikom uvoza tablice. Zadani broj istovremene operacije rada jednak je broj procesora core; Međutim, možete odrediti različit broj Istodobni s mogućnošću `/NC`. Dodatne informacije, upišite `AzCopy /?:NC` u naredbenom retku.

Imajte na umu da AzCopy podržava samo uvoz za JSON, ne CSV. AzCopy podržava uvozom tablica iz JSON stvorili kao korisnik i manifest datoteke. Obje se datoteke mora potjecati iz tablice izvezite AzCopy. Da biste izbjegli pogreške, provjerite ne mijenjajte izvezene JSON i datoteku manifesta.

### <a name="import-entities-to-table-using-blobs"></a>Uvoz entiteti u tablici pomoću blob-ova

Pretpostavimo spremniku Blob sadrži sljedeće: datoteka odgovora JSON predstavljaju tablice programa Azure i njegove popratnu manifesta datoteke.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Možete pokrenite sljedeću naredbu da biste uvezli entiteti u tablicu pomoću manifesta datoteke u njih blob:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"

## <a name="other-azcopy-features"></a>Ostale značajke AzCopy

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Samo kopirate podatke koji ne postoji odredište

Na `/XO` i `/XN` parametara omogućuju vam da biste izuzeli resursi starije ili novija verzija izvora iz koja se kopira, odnosno. Ako samo želite kopirati izvor resursa koje ne postoje u odredište, možete odrediti oba parametra AzCopy naredba:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Napomena: To nije podržan kada je izvorne ili odredišne tablice.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Korištenje datoteke odgovora da biste odredili parametara naredbenog retka

    AzCopy /@:"C:\responsefiles\copyoperation.txt"

Možete uključiti parametre naredbenog retka AzCopy u datoteci odgovora. AzCopy procesa parametara u datoteku kao da ste nije naveden u naredbenom retku izvođenje Izravni zamjena sa sadržajem datoteke.

Pretpostavimo odgovor datoteku pod nazivom `copyoperation.txt`, koja sadrži sljedeće retke. Možete navesti parametra AzCopy u jednom retku

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

i dalje razdvojiti retke:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy neće uspjeti ako podijelite parametar preko dva retka, kao što je prikazano ovdje za na `/sourcekey` parametar:

    http://myaccount.blob.core.windows.net/mycontainer
    C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Korištenje većeg broja odgovora datoteka da biste odredili parametara naredbenog retka

Pretpostavimo odgovor datoteku pod nazivom `source.txt` koji određuje spremniku izvora:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

I odgovor datoteku pod nazivom `dest.txt` koji određuje odredišnu mapu u datotečnom sustavu:

    /Dest:C:\myfolder

I odgovor datoteku pod nazivom `options.txt` koji određuje mogućnosti za AzCopy:

    /S /Y

Da biste nazvali AzCopy s tim datotekama odgovor, koji se nalaze u direktoriju `C:\responsefiles`, koristite sljedeću naredbu:

    AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

AzCopy obrađuje tu naredbu, baš kao i ga ako ste obuhvatili sve parametre pojedinačne u naredbenom retku:

    AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

### <a name="specify-a-shared-access-signature-sas"></a>Odredite zajednički pristup potpis (SAS)

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

Možete odrediti i SAS na spremnik URI:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

### <a name="journal-file-folder"></a>Mapa u dnevnik

Svaki put kada pokrećete naredbu za AzCopy, ga provjerava postoji li datoteka dnevnik u zadanu mapu ili je li ona se nalazi u mapi koju ste naveli putem tu mogućnost. Ako datoteke dnevnika ne postoji na svakom mjestu, AzCopy smatra postupka nove i stvara novu datoteku dnevnika.

Ako postoji datoteke dnevnika, AzCopy će provjerite podudara li naredbenog retka koji vam unos naredbenog retka u datoteci dnevnika. Ako se podudaraju s dva retka naredbe, AzCopy nastavlja nepotpuna operacija. Ako se ne podudaraju, zatražit će se ili prebrisati dnevnik datoteku da biste započeli novu radnju ili da biste odustali trenutne operacije.

Ako želite koristiti zadano mjesto datoteke dnevnika:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

Ako izostavite mogućnost `/Z`, ili navedite mogućnost `/Z` bez put do mape kao što je prikazano gore, AzCopy stvara dnevnik datoteku na zadano mjesto, što je `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Ako već postoji datoteka dnevnik, AzCopy nastavlja operacije na temelju datoteke dnevnika.

Ako želite da biste odredili prilagođene mjesto spremanja datoteke dnevnika:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

U ovom se primjeru stvara datoteke dnevnika ako se još ne postoji. Ako postoji AzCopy nastavlja operacije na temelju datoteke dnevnika.

Ako želite da biste nastavili postupak AzCopy:

    AzCopy /Z:C:\journalfolder\

U ovom se primjeru nastavlja posljednju operaciju koja je neuspješna da biste dovršili.

### <a name="generate-a-log-file"></a>Stvaranje datoteke zapisnika

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

Ako navedete mogućnost `/V` bez osiguravanja put datoteke opširno zapisnik, zatim AzCopy stvara datoteku zapisnika zadano mjesto, što je `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

U suprotnom, možete stvoriti prilagođene lokacije u datoteci zapisnika:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

Imajte na umu da navedete relativni put pratiti mogućnost `/V`, kao što su `/V:test/azcopy1.log`, a zatim opširno zapisnika se stvara u trenutnom radnom direktoriju u podmapu s nazivom `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Navedite broj istovremene operacije da biste započeli

Mogućnost `/NC` određuje broj operacije Istodobni kopiranja. Prema zadanim postavkama AzCopy pokreće određenog broja istovremene operacije da biste povećali propusnost prijenos podataka. Za tablicu operacije, broj istovremene operacije je jednako broja procesora koji imate. Operacija Blob i datoteke, broj istovremene operacije je jednako 8 puta broja procesora koji imate. Ako koristite AzCopy putem mreže niske propusnosti, možete odrediti manji broj za /NC za izbjegavanje problema uzrokovanih konkurencije resursa.

### <a name="run-azcopy-against-azure-storage-emulator"></a>Pokrenuti AzCopy Emulator Azure prostora za pohranu

Možete pokrenuti AzCopy protiv [Emulator Azure prostora za pohranu](storage-use-emulator.md) za blob-ova:

    AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

i tablice:

    AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table

## <a name="azcopy-parameters"></a>Parametri AzCopy

Slijedi opis parametara za AzCopy. Možete upisati i jednu od sljedećih naredbi iz naredbenog retka za pomoć u korištenju AzCopy:

- Podrobne upute naredbenog retka za AzCopy:`AzCopy /?`
- Podrobne upute s bilo kojeg parametar AzCopy:`AzCopy /?:SourceKey`
- Primjer naredbenog retka:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Izvora: "izvor"

Određuje izvorišnih podataka iz koje želite kopirati. Izvorišnog web-mjesta može biti direktorij sustava datoteka, spremniku blob, blob virtualnog direktorija, zajedničke datoteke za pohranu, direktorij prostora za pohranu datoteka ili Azure tablice.

**Odnosi se na:** Blob-ova, datoteke, tablica

### <a name="destdestination"></a>/ Odredišne: "odredište"

Određuje odredište za kopiranje. Odredište može biti direktorij sustava datoteka, spremniku blob, blob virtualnog direktorija, zajedničke datoteke za pohranu, direktorij prostora za pohranu datoteka ili Azure tablice.

**Odnosi se na:** Blob-ova, datoteke, tablica

### <a name="patternfile-pattern"></a>/ Uzorka: "datoteka uzorak"

Određuje uzorak datoteku koja pokazuje koje datoteke da biste kopirali. Ponašanje /Pattern parametar određuje mjesto izvorišnih podataka i prisutnosti mogućnost način rekurzivne. Rekurzivne način naveden putem mogućnosti /S.

Ako navedeni izvor direktorij u datotečnom sustavu, zatim standardne zamjenskih znakova na snazi su, a uzorak datoteke navedene se uparuje s datotekama u direktoriju. Ako mogućnost /S nije naveden, zatim AzCopy udovoljava i navedeni uzorak protiv sve datoteke u sve podmape ispod imenika.

Ako je navedena izvor blob spremnik ili virtualnog direktorija, zatim zamjenskih znakova se ne primjenjuju. Ako mogućnost /S nije naveden, zatim AzCopy interpretira uzorak navedenu datoteku kao blob prefiks. Ako mogućnost /S nije naveden, zatim AzCopy udovoljava uzorak datoteku na temelju imena točna blob.

Ako je navedena izvor Azure zajedničkom, a zatim navedite naziv točno datoteke (npr. abc.txt) da biste kopirali u jednu datoteku, ili odredite mogućnost /S da biste kopirali sve datoteke u rekurzivno zajedničko korištenje. Pokušaj da biste odredili uzorak datoteke i mogućnosti /S zajedno uzrokovat će pogrešku.

AzCopy koristi velika i mala slova podudaranje kada u /Source je spremnik blob ili blob virtualnog direktorija, a koristi velika i mala slova koji se podudaraju u svim drugim slučajevima.

Zadani uzorak datoteka koristi kada nema uzorak datoteka je navedena je *.* mjesto sustava datoteka ili prazan prefiks za neko mjesto za pohranu Azure. Određivanje uzoraka više datoteka nisu podržani.

**Odnosi se na:** Blob-ova, datoteka

### <a name="destkeystorage-key"></a>/ DestKey: "-ključa za pohranu"

Određuje ključ računa za pohranu za resurs odredište.

**Odnosi se na:** Blob-ova, datoteke, tablica

### <a name="destsassas-token"></a>/ DestSAS: "sas token"

Određuje zajednički pristup potpis (SAS) s dozvolama za ČITANJE i PISANJE odredišta (Ako je primjenjivo). Smjestite SAS s dvostrukih navodnika, kao što je možda sadrži posebne znakove naredbenog retka.

Ako je odredište resursa blob kontejneru, zajedničko korištenje datoteka ili tablice, tu mogućnost slijedi SAS token ili možete odrediti ili možete odrediti na SAS kao dio blob spremnik odredište, zajedničko korištenje datoteka ili URI tablice, bez tu mogućnost.

Ako izvorišni i odredišni su oba blob-ova, blob odredište mora se nalaziti unutar iste račun za pohranu kao blob izvora.

**Odnosi se na:** Blob-ova, datoteke, tablica

### <a name="sourcekeystorage-key"></a>/ SourceKey: "-ključa za pohranu"

Određuje ključ računa za pohranu za resurs izvora.

**Odnosi se na:** Blob-ova, datoteke, tablica

### <a name="sourcesassas-token"></a>/ SourceSAS: "sas token"

Određuje potpis programa Access za zajedničko korištenje dozvola za ČITANJE i popis izvora (Ako je primjenjivo). Smjestite SAS s dvostrukih navodnika, kao što je možda sadrži posebne znakove naredbenog retka.

Ako izvor resurs je spremnik blob, uvjet, a ključ ni SAS, kontejner s blob će čitati putem anonimni pristup.

Ako je izvor za zajedničko korištenje datoteka ili tablici, ključ ili SAS mora se navesti.

**Odnosi se na:** Blob-ova, datoteke, tablica

### <a name="s"></a>/S

Određuje način rekurzivne za operacije kopiranja. U načinu rekurzivne AzCopy će kopirati sve blob-ova ili datoteke koje zadovoljavaju navedene datoteke uzorka, uključujući one u podmape.

**Odnosi se na:** Blob-ova, datoteka

### <a name="blobtypeblock--page--append"></a>/ BlobType: "blok" | "stranica" | "Dodaj"

Navodi je li odredište blob blob blok, blob stranice ili je blob dodavanjem. Ta je mogućnost primjenjiva samo kada prenosite blob. U suprotnom, generirat ćete pogrešku. Ako je odredište blob, a ta mogućnost nije naveden, po zadanom AzCopy stvara blob blok.

**Odnosi se na:** Blob-ova

### <a name="checkmd5"></a>/ CheckMD5

Izračunava se raspršivanje MD5 preuzete podataka i potvrđuje da raspršivanje MD5 pohranjene u blob-om ili njezin sadržaj MD5 svojstvo odgovara izračunatog raspršivanje. Provjera MD5 je po zadanome isključen, tako da odredite tu mogućnost da provjeri MD5 prilikom preuzimanja podataka.

Imajte na umu da Azure prostora za pohranu ne jamči ažuran raspršivanje MD5 pohranjene blob ili datoteke. To je odgovornosti klijentskog računala da biste ažurirali na MD5 kad god je izmijenjena blob ili datoteke.

AzCopy uvijek postavlja svojstva sadržaja MD5 blobova platforme Azure ili datoteke nakon prenošenja sa servisom.  

**Odnosi se na:** Blob-ova, datoteka

### <a name="snapshot"></a>/ Snimke

Označava hoće li se prijenos snimke. Ova mogućnost je valjan samo kad je izvor blob.

Snimke prenesene blob preimenuju u ovom obliku: .extension blob-naziv (snimke vrijeme)

Prema zadanim postavkama snimaka ne kopiraju.

**Odnosi se na:** Blob-ova

### <a name="vverbose-log-file"></a>/ V: [opširno-datoteke zapisnika]

Izlaze poruke o statusu opširno u datoteke zapisnika.

Prema zadanim postavkama opširno zapisničke datoteke pod nazivom AzCopyVerbose.log u `%LocalAppData%\Microsoft\Azure\AzCopy`. Ako odredite mjesto postojećeg datoteke za tu mogućnost, opširno zapisnika će se dodati tu datoteku.  

**Odnosi se na:** Blob-ova, datoteke, tablica

### <a name="zjournal-file-folder"></a>/ Z: [dnevnik--mapa]

Navodi mapu dnevnik datoteke za nastavak operacije.

AzCopy uvijek podržava nastavljanje ako prekinuta je postupak.

Ako ta mogućnost nije navedena ili navedena je bez put do mape, zatim AzCopy će stvoriti datoteke dnevnika zadano mjesto, što je % LocalAppData%\Microsoft\Azure\AzCopy.

Svaki put kada pokrećete naredbu za AzCopy, ga provjerava postoji li datoteka dnevnik u zadanu mapu ili je li ona se nalazi u mapi koju ste naveli putem tu mogućnost. Ako datoteke dnevnika ne postoji na svakom mjestu, AzCopy smatra postupka nove i stvara novu datoteku dnevnika.

Ako postoji datoteke dnevnika, AzCopy će provjerite podudara li naredbenog retka koji vam unos naredbenog retka u datoteci dnevnika. Ako se podudaraju s dva retka naredbe, AzCopy nastavlja nepotpuna operacija. Ako se ne podudaraju, zatražit će se ili prebrisati dnevnik datoteku da biste započeli novu radnju ili da biste odustali trenutne operacije.

Dnevnik datoteka izbriše nakon uspješan dovršetak postupka.

Imajte na umu da nastavljanje postupak iz datoteke dnevnika stvorenu u starijoj verziji programa AzCopy nije podržana.

**Odnosi se na:** Blob-ova, datoteke, tablica

### <a name="parameter-file"></a>/@:"parameter-file"

Određuje datoteku koja sadrži parametara. AzCopy procesa parametara u datoteci, baš kao da ste nije naveden u naredbenom retku.

U datoteci odgovora, možete navesti više parametara u jednom retku, ili odredite parametra zasebnom retku. Imajte na umu pojedinačne parametar ne može obuhvaćati više redaka.

Datoteka odgovora možete uključiti linija komentare koje počinju znak #.

Možete navesti više datoteka odgovor. Međutim, imajte na umu da ne podržava AzCopy ugniježđene datoteke odgovora.

**Odnosi se na:** Blob-ova, datoteke, tablica

### <a name="y"></a>/ Y

Ukida sve AzCopy upute za potvrdu.

**Odnosi se na:** Blob-ova, datoteke, tablica

### <a name="l"></a>/L

Određuje operacije na stavci samo; Nema podaci kopiraju se.

AzCopy tumačenje na pomoću ove mogućnosti kao simulacije za pokretanje naredbenog retka bez to option /L i brojanja koliko objekte koji se kopiraju, možete odrediti mogućnost /V u isto vrijeme da biste provjerili koji objekti kopirat će se u zapisniku opširno.

Ponašanje tu mogućnost i određen mjesto izvorišnih podataka i prisutnosti rekurzivne način mogućnost /S datoteka uzorak mogućnost i /Pattern.

AzCopy potrebna je dozvola popisa i ČITANJE to mjesto izvora prilikom korištenja tu mogućnost.

**Odnosi se na:** Blob-ova, datoteka

### <a name="mt"></a>/MT

Postavlja preuzetu datoteku posljednji izmijenio vremena da je isti kao izvor blob ili datoteke.

**Odnosi se na:** Blob-ova, datoteka

### <a name="xn"></a>/XN

Isključuje novija resursa izvora. Resurs će se kopirati ako je vrijeme zadnje izmjene izvora isti ili novija verzija od odredište.

**Odnosi se na:** Blob-ova, datoteka

### <a name="xo"></a>/XO

Isključuje starije resursa za izvor. Resurs će se kopirati ako je vrijeme zadnje izmjene izvora isti ili stariji od odredište.

**Odnosi se na:** Blob-ova, datoteka

### <a name="a"></a>/A

Prenosite samo datoteke atribut arhiva postavljen.

**Odnosi se na:** Blob-ova, datoteka

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]

Prenosite samo datoteke koje se neki skupa navedeni atributi.

Dostupni atributi obuhvaćaju sljedeće:

- R = datoteke samo za čitanje
- A = datoteke spreman za arhiviranje
- S = sistemske datoteke
- H = skrivene datoteke
- C = komprimirane datoteke
- N = normalno datoteke
- E = šifrirana datoteka
- T = privremene datoteke
- O = izvanmrežne datoteke
- I = osobe koje nisu indeksirana datoteke

**Odnosi se na:** Blob-ova, datoteka

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]

Isključuje datoteke koje imaju bilo koji od skup navedeni atributi.

Dostupni atributi obuhvaćaju sljedeće:

- R = datoteke samo za čitanje
- A = datoteke spreman za arhiviranje
- S = sistemske datoteke
- H = skrivene datoteke
- C = komprimirane datoteke
- N = normalno datoteke
- E = šifrirana datoteka
- T = privremene datoteke
- O = izvanmrežne datoteke
- I = osobe koje nisu indeksirana datoteke

**Odnosi se na:** Blob-ova, datoteka

### <a name="delimiterdelimiter"></a>/ Graničnik: "graničnik"

Označava znak razdjelnika koji se koristi za razdijelite virtualne direktorije blob naziv.

Po zadanom koristi AzCopy / kao znak razdjelnika. Međutim, AzCopy podržava sve uobičajene znak (kao što su @, # ili %) kao razdjelnika. Ako vam je potrebna jednu od ovih posebnih znakova u naredbenom retku, stavite naziv datoteke s dvostrukih navodnika.

Ta je mogućnost samo za preuzimanje blob-ova.

**Odnosi se na:** Blob-ova

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "broj-od-istovremene-operacije"

Određuje broj istovremene operacije.

AzCopy po zadanom se pokreće određenog broja istovremene operacije da biste povećali propusnost prijenos podataka. Imajte na umu velik broj istovremene operacije u okruženju niske propusnosti može pretrpati mrežne veze i spriječili operacije potpuno dovršavanje. Throttle istovremene operacije temelju stvarni dostupna propusnost mreže.

Gornja granica za istovremene operacije je 512.

**Odnosi se na:** Blob-ova, datoteke, tablica

### <a name="sourcetypeblob--table"></a>/ SourceType: "Blob" | "Tablice"

Određuje koji se `source` resursa je dostupna u lokalnom razvojno okruženje, izvodi u emulator prostora za pohranu blob.

**Odnosi se na:** Blob polja tablice

### <a name="desttypeblob--table"></a>/ DestType: "Blob" | "Tablice"

Određuje koji se `destination` resursa je dostupna u lokalnom razvojno okruženje, izvodi u emulator prostora za pohranu blob.

**Odnosi se na:** Blob polja tablice

### <a name="pkrskey1key2key3"></a>/ PKRS: "ključ1 # ključ2 # ključ3 #..."

Dijeli raspon particija ključa da biste omogućili Izvoz tablice podataka paralelno, što povećava brzinu izvoza.

Ako ta mogućnost nije naveden, AzCopy jedan niti koristi za izvoz tablice entiteti. Na primjer, ako korisnik navede /PKRS: "aa #bb", a zatim AzCopy pokreće tri istovremene operacije.

Svaka operacija izvozi neku od tri raspona ključa particije, kao što je prikazano u nastavku:

  [prvog particija ključa, aa)

  [aa, bb)

  [bb zadnjem particija ključa]

**Odnosi se na:** Tablica

### <a name="splitsizefile-size"></a>/ SplitSize: "veličina datoteke"

Određuje izvezenu datoteku Podjela veličina u MB, minimalna dopuštena vrijednost jest 32.

Ako ta mogućnost nije naveden, AzCopy izvezite podatke iz tablice u jednu datoteku.

Ako blob izvoza podatke u tablici, a veličina izvezene datoteke je dostigne 200 GB ograničenje veličine blob, zatim AzCopy će podijeliti izvezenu datoteku, čak i ako se ta mogućnost nije naveden.

**Odnosi se na:** Tablica

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"

Određuje ponašanje Uvoz podatkovne tablice.

- InsertOrSkip – preskače postojeći entitet ili umeće novi entitet, ako ne postoji u tablici.

- InsertOrMerge - spajanja postojeći entitet ili umeće novi entitet, ako ne postoji u tablici.

- InsertOrReplace - zamjenjuje postojeći entitet ili umeće novi entitet, ako ne postoji u tablici.

**Odnosi se na:** Tablica

### <a name="manifestmanifest-file"></a>/ Manifesta: "manifest datoteke"

Određuje datoteka manifesta za tablicu izvoz i uvoz operacija.

Ta je mogućnost neobavezni tijekom postupka izvoza, AzCopy generirat će manifesta datoteka unaprijed definiranih naziva ako ta mogućnost nije naveden.

Ta je mogućnost potrebna tijekom operacije uvoza za pronalaženje podatkovnih datoteka.

**Odnosi se na:** Tablica

### <a name="synccopy"></a>/ SyncCopy

Označava hoće li se sinkronizirano kopirajte blob-ova ili datoteke između dva Azure prostora za pohranu krajnje točke.

AzCopy po zadanom koristi poslužiteljsko asinkronog kopiju. Navedite tu mogućnost da biste izvršili kopiju sinkrono koji preuzima blob-ova ili datoteka na lokalni memorije i prenosi ih Azure prostora za pohranu.

Tu mogućnost možete koristiti prilikom kopiranja datoteke iz spremišta blobova unutar pohrani ili iz spremišta blobova pohrani ili obrnuto.

**Odnosi se na:** Blob-ova, datoteka

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "vrsta sadržaja"

Navodi vrstu sadržaja MIME odredište blob-ova ili datoteka.

AzCopy po zadanom postavlja vrste sadržaja za blob ili datoteka aplikacije/octet-strujanje. Vrste sadržaja za sve blob-ova ili datoteka možete postaviti tako da izričito vrijednost za tu mogućnost.

Ako navedete tu mogućnost bez vrijednosti, AzCopy će postaviti svaki blob ili vrstu sadržaja datoteke prema njezin datotečni nastavak.

**Odnosi se na:** Blob-ova, datoteka

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" | "CSV"

Određuje oblik datoteke izvezene podatkovne tablice.

Ako ta mogućnost nije naveden, po zadanom AzCopy Izvoz tablice podatkovne datoteke u obliku JSON.

**Odnosi se na:** Tablica

## <a name="known-issues-and-best-practices"></a>Poznati problemi i najbolje prakse

### <a name="limit-concurrent-writes-while-copying-data"></a>Ograničenje Istodobni zapisivanja prilikom kopiranja podataka

Kada kopirate blob-ova ili datoteke s AzCopy, imajte na umu da druge aplikacije mogu biti izmjena podataka dok je kopirate. Ako je to moguće, provjerite je li da kopirate podatke je onemogućiti izmjenu tijekom postupka Kopiraj. Ako, na primjer, pri kopiranju VHD pridružene Azure virtualnog računala, provjerite da nema drugih aplikacija trenutno pišete, da biste na VHD. Dobar način za to je lizing resursa koje želite kopirati. Također, prvo stvorite snimke u VHD, a zatim kopirajte snimke.

Ako ne možete spriječiti drugim aplikacijama iz pisanja blob-ova ili datoteke dok su koja se kopira, zatim Imajte na umu da prema vremenu posao završi, kopirani resurse možda neće imati cijeli slične značajke s resursima za izvor.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Pokrenite jedan instancu AzCopy na jednom računalu.
AzCopy namijenjen je Maksimiziranje Upotreba vaše računalo resursa za ubrzano prijenos podataka, preporučujemo da se pokrenuti samo jedna instanca AzCopy na jednom računalu i navedite mogućnost `/NC` ako trebate više istovremene operacije. Dodatne informacije, upišite `AzCopy /?:NC` u naredbenom retku.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>Omogućivanje FIPS usklađen MD5 algoritama za AzCopy kada vam "koristi FIPS usklađen algoritama za šifriranje, raspršivanje i potpisivanje".
AzCopy po zadanom koristi .NET MD5 implementacije za izračun u MD5 pri kopiranju objekata, no postoje neke sigurnosne preduvjete koje je potrebno AzCopy da biste omogućili FIPS usklađen MD5 postavku.

Možete stvoriti datoteku app.config `AzCopy.exe.config` s svojstvo `AzureStorageUseV1MD5` te ih aside s AzCopy.exe.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

• Svojstvo "AzureStorageUseV1MD5" True - zadane vrijednosti, AzCopy će koristiti .NET MD5 implementacije.
• False – AzCopy će koristiti FIPS usklađen MD5 algoritam.

Imajte na umu da je po zadanom na računalu Windows onemogućeni FIPS usklađen algoritama, možete upisati secpol.msc u prozoru Pokreni i potvrdite taj parametar pri odabrana postavka sigurnosti -> Lokalna pravila -> sigurnost mogućnosti -> sustav šifriranja: korištenje FIPS usklađen algoritama za šifriranje, raspršivanje i prijava.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o Azure prostora za pohranu i AzCopy potražite u sljedećim člancima.

### <a name="azure-storage-documentation"></a>Azure dokumentacija za pohranu:

- [Uvod u Azure prostora za pohranu](storage-introduction.md)
- [Upute za korištenje spremišta blobova iz .NET](storage-dotnet-how-to-use-blobs.md)
- [Kako koristiti za pohranu datoteka iz .NET](storage-dotnet-how-to-use-files.md)
- [Upute za korištenje spremišta tablica iz .NET](storage-dotnet-how-to-use-tables.md)
- [Upute za stvaranje, upravljanje i brisanje računa za pohranu](storage-create-storage-account.md)

### <a name="azure-storage-blog-posts"></a>Azure prostora za pohranu članaka za blog:
- [Uvod u biblioteke Preview za Azure prostora za pohranu podataka premještanja](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
- [AzCopy: Uvod u korištenje sinkrono kopiju i prilagođene vrste sadržaja](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Objavljivanje Općenito dostupnost od AzCopy 3.0 plus pretpregled izdanja AzCopy 4.0 s podrškom za tablice i datoteke](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Optimiziran za veliki Kopiraj scenariji](http://go.microsoft.com/fwlink/?LinkId=507682)
- [AzCopy: Podrška za pristup za čitanje prostora za pohranu suvišne zemlj.](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy: Prijenos podataka s može ponovno pokrenuti način rada i SAS tokena](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy: Pomoću više računa Kopiraj Blob](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy: Prijenos i preuzimanje datoteka za Azure blob-ova](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)
