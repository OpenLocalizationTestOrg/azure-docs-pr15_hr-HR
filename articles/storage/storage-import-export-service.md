<properties
    pageTitle="Korištenje uvoz/izvoz za prijenos podataka sa spremištem blobova | Microsoft Azure"
    description="Saznajte kako stvoriti uvoz i izvoz zadatke na portalu za Azure klasični za prijenos podataka da biste bloba prostora za pohranu."
    authors="renashahmsft"
    manager="aungoo"
    editor="tysonn"
    services="storage"
    documentationCenter=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="renash"/>


# <a name="use-the-microsoft-azure-importexport-service-to-transfer-data-to-blob-storage"></a>Prijenos podataka sa spremištem blobova pomoću servisa Microsoft Azure uvoz/izvoz

## <a name="overview"></a>Pregled

Azure servis za uvoz/izvoz omogućuje siguran prijenos velikih količina podataka blobova platforme Azure po dostavu tvrdom disku pogona centar za Azure podataka. Možete koristiti i taj servis prenijeti podatke s blobova platforme Azure na tvrdom disku pogona i isporuku lokalno mjesto. Taj servis je odgovarajuću u slučajevima gdje želite prenijeti nekoliko TBs podataka u ili iz Azure, ali prijenos ili preuzimanje putem mreže nije izvedivo zbog ograničeni propusnosti ili troškove visoke mreže.

Servis potrebna je tvrdi diskovnih pogona mora biti bitne ormarić šifrirana za sigurnost vaših podataka. Servis podržava klasični prostora za pohranu računa koje su prisutne u svim regijama javno Azure. Moraju postaviti tvrdi diskovnih pogona za podržane mjesta navedena u nastavku ovog članka.

U ovom se članku će Saznajte više o servisa Azure uvoz/izvoz i upute za isporuku pogona za kopiranje podataka i iz spremišta blobova Azure.

> [AZURE.IMPORTANT] Stvaranje i upravljanje uvoz i izvoz poslova klasični pomoću portala za klasični ili [servis za uvoz/izvoz REST API -ji](http://go.microsoft.com/fwlink/?LinkID=329099)za pohranu. Voditelj resursa za pohranu račune trenutno nisu podržani.

## <a name="when-should-i-use-the-azure-importexport-service"></a>Kada koristiti servis za uvoz/izvoz Azure?

Razmotrite korištenje servisa Azure uvoz/izvoz prilikom prijenosa ili previše sporo se preuzima podataka putem mreže ili početak propusnost mreže dodatna je trošak prohibitive.

Možete koristiti taj servis u slučajevima kao što su:

- Prijenos podataka s oblakom: premještanje velikih količina podataka Azure brzo i učinkovito.
- Sadržaj raspodjele: brzo slanje podataka u web-mjesta za korisnike.
- Sigurnosna kopija: Potrebno sigurnosne kopije lokalnih podataka za pohranu u spremište blobova platforme Azure.
- Oporavak podataka: oporavak veliku količinu podataka pohranjenih u spremište blobova platforme, a da se isporučuju lokalne lokacije.

## <a name="pre-requisites"></a>Prije requisites

U ovom ćete odjeljku smo popisu stara requisites potrebne za korištenje ove usluge. Pregledajte ih pažljivo prije dostavu pogonima.

### <a name="storage-account"></a>Račun za pohranu

Morate imati postojeću pretplatu na Azure i jedan ili više **Klasični** prostora za pohranu račune za korištenje usluge uvoz/izvoz. Svaki zadatak može se koristiti za prijenos podataka u ili iz samo jedan račun klasični prostora za pohranu. Drugim riječima, ne može obuhvaćati posao jedan uvoz/izvoz u više računa za pohranu. Informacije o stvaranju novog računa za pohranu potražite u članku [Stvaranje računa za pohranu](storage-create-storage-account.md#create-a-storage-account).

### <a name="blob-types"></a>Vrste blobova platforme

Da biste kopirali podatke **bloka** blob-ova ili blob-ova **stranica** pomoću servisa Azure uvoz/izvoz. Nasuprot tome, možete izvoziti samo **bloka** blob-ova, blob-ova **stranica** ili blob **dodavanjem** polja iz Azure pomoću ove usluge za pohranu.

### <a name="job"></a>Zadatak

Da biste započeli postupak uvoz ili izvoz iz spremišta blobova, najprije stvorite zadatak. Posao može biti posao uvoza ili je zadatak izvoza:

- Stvorite posao uvoza kad želite prenijeti podatke imate lokalni da biste blob-ova u račun za Azure prostora za pohranu.
- Stvorite zadatak programa izvoza kad želite prenijeti podatke koji su trenutno spremljene kao BLOB-ova u vašem računu za pohranu da biste napravili pogone koje su poslane.

Kada stvorite zadatak, ćete obavijestiti servis za uvoz/izvoz da koje će biti uključena jedan ili više tvrdi pogona centar za Azure podataka.

- Za posao uvoza, će dostavu tvrdog diska koja sadrži vaše podatke.
- Za zadatak programa izvoza će dostavu prazan tvrdog diska.
- Do 10 tvrdom diskovnih pogona po poslu možete isporuka.

Možete stvoriti uvoza ili izvoza posla pomoću [portala za klasični](https://manage.windowsazure.com/) ili [Azure prostora za pohranu uvoz/izvoz REST API -JA](http://go.microsoft.com/fwlink/?LinkID=329099).

### <a name="client-tool"></a>Alat za klijenta

Prvi korak pri stvaranju posao za **Uvoz** je Priprema na pogon koji će biti isporučeni za uvoz. Da biste pripremili na pogon, morate povezati s lokalnog poslužitelja i pokrenite alat za klijent uvoz/izvoz Azure na lokalni poslužitelj. Ovaj alat za klijent olakšava kopiranje podataka na pogon, podatke na pogonu koji i generiranje datoteke dnevnika pogon.

Dnevnik datoteke pohranite osnovne informacije o zadatak i pogon kao što su pogon serijski broj i naziv računa za pohranu. Ove datoteke dnevnika nisu spremljeni na disku. Koristi se tijekom stvaranja posla uvoza. Korak po korak detalje o posao stvaranja su navedeni u nastavku ovog članka.

Alat za klijenta samo kompatibilan sa 64-bitni operacijski sustav Windows. OS verzije podržane potražite u odjeljku [operacijski sustav](#operating-system) .

Preuzmite najnoviju verziju [Azure uvoz/izvoz alata za klijenta](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409). Dodatne informacije o korištenju alata za uvoz/izvoz Azure potražite u članku [Referenca za Azure uvoz/izvoz alata](http://go.microsoft.com/fwlink/?LinkId=329032).

### <a name="hard-disk-drives"></a>Tvrdi disk pogona

3,5 inča SATA II III internog tvrdog diska podržani su za korištenje sa servisom za uvoz/izvoz. Možete koristiti tvrdog diska do 10TB.
Za poslove uvoza samo prvi podataka glasnoće na disku obrađuju. Glasnoću podataka mora biti oblikovana s NTFS.
Kada kopiranje podataka na tvrdom disku, možete ih priložiti izravno pomoću poveznika SATA ili možete priložiti vanjsko s vanjskim prilagodnik za USB II/III SATA. Preporučujemo da koristite neku od sljedećih prilagodnici vanjski USB SATA II/III:

- Anker 68UPSATAA - 02 Grafičke
- 68UPSHHDS Grafičke Anker
- Startech SATADOCK22UE
- Sharkoon QuickPort XT HC

Ako imate pretvornika koji nije naveden, pokušajte pokretanja alata za uvoz/izvoz Azure pomoću svoje pretvornika za pripremu pogon i potražite u članku funkcionira prije kupnje pretvornika podržane.

> [AZURE.IMPORTANT] Vanjski tvrdi diskovnih pogona koji se isporučuju s ugrađene prilagodnik za USB ne podržava taj servis. Na disku unutar kućište vanjskih podizanje tvrdog diska ne može se koristiti; Pošaljite vanjski HDDs.

### <a name="encryption"></a>Šifriranje

Podaci na pogonu mora biti šifriran pomoću BitLocker šifriranje pogona. Time se štite podataka dok je na putu.

Za uvoz poslove, dva su načina za šifriranje. Prvi način je da biste koristili na / šifriranje parametar prilikom pokretanja alata za klijent tijekom pripreme pogon. Drugi način je da biste omogućili BitLocker šifriranje ručno na disku i navedite ključa za šifriranje u naredbenom retku klijentskih alata tijekom pripreme pogon.

Za izvoz poslove, kada se vaši podaci kopiraju u pogona, servis će šifrirati pogon koristeći BitLocker prije dostavu natrag. Ključ za šifriranje objavit ćemo zajedno vam putem klasični portal.  

### <a name="operating-system"></a>Operacijski sustav

Da biste pripremili tvrdi disk pomoću alata za uvoz/izvoz Azure prije dostavu pogon za Azure možete koristiti jedan od sljedećih 64-bitne operacijske sustave:

Windows 7 Enterprise, Windows 7 Ultimate Windows 8 Pro, tvrtke za Windows 8, Windows 8.1 Pro, tvrtke za Windows 8.1, Windows 10<sup>1</sup>, Windows Server 2008 R2 sustava Windows Server 2012, Windows Server 2012 R2. Sve te operacijske sustave podržava BitLocker šifriranje pogona.

> [AZURE.IMPORTANT] <sup>1</sup> Ako koristite Windows 10 stroj da biste pripremili tvrdog diska, preuzmite najnoviju verziju programa Azure alat za uvoz/izvoz.

### <a name="locations"></a>Mjesta

Servis za uvoz/izvoz Azure podržava kopiranje podatke iz svi računi javno Azure prostora za pohranu. Možete isporuka tvrdom disku pogona na neki od sljedećih mjesta. Ako je vaš račun za pohranu u javno mjesto za Azure koji nije ovdje navedeno, na mjesto zamjenski dostave objavit ćemo zajedno prilikom stvaranja posla pomoću portala za klasični ili uvoz/izvoz REST API-JA.

Podržani dostave mjesta:

- Istočni SAD-a

- Zapad SAD-a

- Istočni sad 2

- Središnje SAD-a

- Sjeverna središnje SAD-a

- Južna središnje SAD-a

- Sjeverna Europa

- Europa Zapad

- Istočnoazijski

- Jugoistočne Azije

- Istok Australija

- Australija Jugoistok

- Japan Zapad

- Istok Japan

- Središnje Indija


### <a name="shipping"></a>Dostave

**Pogoni dostave podatkovnog centra u:**

Prilikom stvaranja za uvoz ili izvoz posla, će pružati adresi dostave neke od podržanih lokacija za isporuku pogonima. Dostave adresu koja je navedena ovisi o mjestu vašeg računa za pohranu, ali može biti ista kao mjesto za pohranu računa.

Telefonije kao što su FedEx, DHL, UPS ili poštanski servis NAM možete koristiti za isporuku pogonima na adresi dostave.

**Pogoni dostave iz centra za podataka:**

Prilikom stvaranja za uvoz ili izvoz posao, morate navesti povratnu adresu za Microsoft ćete koristiti prilikom dostavu pogone natrag nakon dovršetka posla. Provjerite je li unesete valjani povratna adresa da biste izbjegli kašnjenja u obrada.

Morate navesti valjani FedEx ili DHL prijenosni računa broj tako da koristi Microsoft za dostavu pogona natrag. Broj računa FedEx nužan za dostavu pogona natrag od mjesta SAD-a i Europe. Broj računa DHL nužan za dostavu pogona natrag od mjesta Azije i Australija. Ako ne postoji možete stvoriti na račun prijenosni [DHL](http://www.dhl.com/) (Azije i Australija) ili [FedEx](http://www.fedex.com/us/oadr/) (za SAD-a i Europe). Ako već imate račun broja prijenosni, provjerite je valjan.

U dostavu vaše paketi moraju pratiti uvjete pri [Uvjete za servis Microsoft Azure](https://azure.microsoft.com/support/legal/services-terms/).

> [AZURE.IMPORTANT] Uzmite u obzir fizičke medijske sadržaje koje ste isporučuju morati Unakrsna međunarodne obrubi. Vi ste odgovorni za osiguravanje da fizičke medijske sadržaje i podaci su uvesti i/ili izvesti u skladu s primjenjivih zakona. Prije dostave fizičke medijskih sadržaja, potvrdite okvir uz vaše advisers da biste potvrdili da medijskih sadržaja i podataka možete legalno otpremljene identificirani podatkovnog centra u. To će pomoći da biste bili sigurni da je dostigne Microsoft pravovremeno. Ako, primjerice, sve paket koji će se presjeći međunarodne obrubi mora komercijalna faktura da biste se praćeni pomoću značajke pakiranja (osim ako presjeka obrube unutar Europske unije). Nije moguće ispisati ispunjeni kopiju komercijalna faktura s prijenosni web-mjesta. Primjer komercijalna faktura su [DHL komercijalna faktura] (http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) ili [FedEx komercijalna faktura](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Pripazite da Microsoft sadrži je označen kao u program za izvoz.

## <a name="how-does-the-azure-importexport-service-work"></a>Kako funkcionira servis za uvoz/izvoz Azure?

Podatke možete se prebaciti između lokalnog web-mjesta i spremište blobova platforme Azure pomoću servisa Azure uvoz/izvoz stvaranjem zadataka i dostavu tvrdom disku pogona centar za Azure podataka. Svakom tvrdom disku vam isporuka povezan je s jedan zadatak. Svaki zadatak povezan je s računom jedan prostora za pohranu. Pročitajte [odjeljak stara requisites](#pre-requisites) pažljivo da biste se informirali o specifičnosti ovaj servis kao što su podržane blob vrste, vrsta diska, mjesta i dostave.

U ovom ćete odjeljku smo opisane visoke razine koraka uvoz i izvoz zadatke. Poslije u [odjeljku brzi početak rada](#quick-start)ćemo vam ponuditi detaljne upute za stvaranje uvoza i izvoza posao.

### <a name="inside-an-import-job"></a>Unutar posao uvoza

Visoke razine posao uvoza obuhvaća sljedeće korake:

- Odredite podatke koje želite uvesti, a broj pogona ćete.
- Odredite odredište blob-ova za svoje podatke u spremište blobova platforme.
- Pomoću alata za uvoz/izvoz Azure da biste kopirali podatke za jednog ili više pogona tvrdom disku i šifriranje BitLocker.  
- Stvorite posao uvoza na vašem računu klasični prostora za pohranu cilj pomoću portala za klasični ili uvoz/izvoz REST API-JA. Ako pomoću portala za klasični, prenesite datoteke dnevnika pogon.
- Navedite povratnu adresu i broj računa Zrakoplovna tvrtka koja će se koristiti za dostavu pogone natrag.
- Isporuka diskovnih pogona tvrdog za dostave adresu koja je navedena tijekom stvaranja posla.
- Ažurirajte isporuke broj u odjeljku uvoz posao detalja za praćenje i slanje poslova uvoza.
- Pogona su primili i obrađuju u središtu Azure podataka.
- Pogona su poslane pomoću računa za prijenosni za povratnu adresu koja je navedena u posao uvoza.

    ![Slika 1:Import zadatak tijeka](./media/storage-import-export-service/importjob.png)


### <a name="inside-an-export-job"></a>Unutar programa zadatak izvoza

Visoke razine na zadatak izvoza obuhvaća sljedeće korake:

- Odredite podatke koje želite izvesti, a broj pogona ćete.
- Odredite izvor blob-ova ili spremnik putove podatke u spremište blobova platforme.
- Stvorite zadatak programa izvoza u račun za pohranu izvora pomoću portala za klasični ili uvoz/izvoz REST API-JA.
- Navedite izvor blob-ova ili spremnik putova podataka u zadatak izvoza.
- Navedite povratnu adresu i broj računa prijenosni za koja će se koristiti za dostavu pogone natrag.
- Isporuka diskovnih pogona tvrdog za dostave adresu koja je navedena tijekom stvaranja posla.
- Ažuriranje isporuku broj u odjeljku Izvoz posao detalja za praćenje i slanje zadatak izvoza.
- Pogone su primili i obrađuju u središtu Azure podataka.
- Pogone šifrirane s BitLocker; tipke su dostupni putem portala za klasični.  
- Pogone su poslane pomoću računa za zrakoplovnu tvrtku za povratnu adresu koja je navedena u posao uvoza.

    ![Slika 2:Export zadatak tijeka](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-status"></a>Pregledavanje statusa zadatka

Možete pratiti stanje uvoza ili izvoza poslove s portala sustava klasični. Idite na račun za pohranu na portalu klasični te kliknite karticu **Uvoz/izvoz** . Na stranici prikazat će se popis poslova. Možete filtrirati popis stanja zadatka, naziv zadatka, vrsta posao ili broj za praćenje.

Vidjet ćete jedan od sljedećih statusa zadatka ovisno o tome gdje je na pogon u postupku.

| Status zadatka   | Opis                                                                                                                                                                                                                                                                                                                                                                        |
|:-------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Stvaranje     | Stvoreno je posla, ali još nije naveden podatke dostavu.                                                                                                                                                                                                                                                                                                |
| Dostave     | Stvoreno je posla, a koju ste naveli podataka za dostavu. **Napomena**: kada pogon se isporučuje u Centar za Azure podatke, status mogu i dalje prikazuju "Dostave" neko vrijeme. Kada je servis pokrene kopirate podatke, status će se promijeniti "Transferring". Ako želite vidjeti točno određene status pogona, poslužite se uvoz/izvoz REST API-JA. |
| Prijenos | Podaci koji se prenose s tvrdog diska (za posao uvoza) ili na tvrdi disk (za zadatak izvoza).                                                                                                                                                                                                                                                                 |
| Pakiranje    | Prijenos podataka bude dovršen, a tvrdog diska Priprema se za dostavu natrag.                                                                                                                                                                                                                                                                             |
| Dovršavanje     | Tvrdi disk je su poslane na vas.                                                                                                                                                                                                                                                                                                                                      |

### <a name="time-to-process-job"></a>Vrijeme za proces zadatka

Koliko dugo traje postupak za uvoz/izvoz posao ovisi o različitim čimbenicima kao što su vrijeme dostave posla vrsta, vrsta i veličina podataka koja se kopira i veličine diskova dobiveni. Servis za uvoz/izvoz nema programa SLA. Da biste pratili tijek zadatka pobliže, poslužite se REST API-JA. Postoji postotka dovršeno parametra u operaciji popis zadataka koja daje naznaku tijeku Kopiraj. Kontaktirajte više osoba odjednom nam ako vam je potrebna Procjena da biste dovršili posao vrijeme ključnih uvoz/izvoz.

### <a name="pricing"></a>Cijene

**Pogon rukovanje naknada**

Postoji naknade za rukovanje pogon za svaki pogon obrađuju kao dio uvoza ili izvoza posao. Pogledajte detalje o [Cijene za Azure uvoz/izvoz](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Troškovi dostave**

Kada isporuka pogona za Azure, plaćate trošak dostave prijenosni dostave. Kada Microsoft vraća pogone vama, trošak dostave dodijeljen prijenosni račun pomoću kojeg koje ste naveli prilikom stvaranja posla.

**Transakcije troškovi**

Postoje bez troškova transakcije prilikom uvoza podataka u spremište blobova platforme. Naknade za standardne izlazne primjenjuju prilikom izvoza podataka iz spremišta blobova. Dodatne informacije o troškovima transakcije potražite u članku [prijenosa podataka cijene.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Brzi početak rada

U ovom ćete odjeljku ćemo vam ponuditi detaljne upute za stvaranje uvoza i na zadatak izvoza. Provjerite zadovoljavaju sve [stara requisites](#pre-requisites) prije prelaska prema naprijed.

## <a name="how-to-create-an-import-job"></a>Kako stvoriti posao uvoza?

Stvorite posao uvoza kopiranje podataka na račun servisa Azure prostora za pohranu iz tvrdog diska po dostavu jedan ili više pogona koji sadrži podatke u Centar za određene podatke. Posao uvoza ostavlja pojedinosti o tvrdom diskovnih pogona, podatke moguće kopirati, ciljani račun za pohranu i dostave informacije sa servisom Azure uvoz/izvoz. Stvaranje posao uvoza se tri koraka. Prvo pripremite pogonima pomoću alata za klijent Azure uvoz/izvoz. Drugo, pošaljite posao uvoza pomoću portala za klasični. Treći, isporuka pogona da biste dostave adresu koja je navedena tijekom stvaranja posla i ažurirati dostave informacije u pojedinosti posao.   

> [AZURE.IMPORTANT] Možete poslati samo jedan zadatak po računu za pohranu. Svaki pogon na koji se isporučuju moguće je uvesti na jedan račun za pohranu. Ako, na primjer, pretpostavimo da želite uvesti podatke u dva računa za pohranu. Morate koristite odvojene tvrdom diskovnih pogona za svaki račun za pohranu i stvorite zaseban zadataka po računu za pohranu.

### <a name="prepare-your-drives"></a>Priprema pogonima

Prvi korak pri uvozu podataka pomoću servisa Azure uvoz/izvoz je da biste pripremili pogonima pomoću alata za klijent Azure uvoz/izvoz. Slijedite korake u nastavku da biste pripremili pogonima.

1.  Odredite podatke koje želite uvesti. To se može biti direktorija i samostalni datoteka na lokalnom poslužitelju ili zajednički mrežni resurs.  

2.  Određivanje broja pogona morat ćete ovisno o ukupnu veličinu podataka. Nabavu potreban broj 3,5 inča SATA II III tvrdom diskovnih pogona.

3.  Odredite ciljnu pohranu račun, spremnik, virtualne direktorije i blob-ova.

4.  Odredite direktorija i/ili samostalne datoteke koje će se kopirati na svakom tvrdom disku.

5.  Pomoću [Alata za uvoz/izvoz Azure](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) da biste kopirali podatke tvrdog diska.

    - Alat za uvoz/izvoz Azure stvara sesije Kopiraj da biste kopirali podatke iz izvora diskovnih pogona tvrdom. U sesiji jednu kopiju alat možete kopirati jedan direktorija uz poddirektorije ili u jednoj datoteci.

    - Možda je potrebno više kopija sesije ako izvora podataka obuhvaća mnogo direktorija.

    - Svakom tvrdom disku pripremite potrebno barem jedan primjerak sesiju.

6.  Možete odrediti na / šifriranje parametar da biste omogućili Bitlocker šifriranje na tvrdom disku. Osim toga, nije moguće i omogućite Bitlocker šifriranje ručno na tvrdom disku i navesti ključ prilikom pokretanja alata.

7.  Alat za uvoz/izvoz Azure stvara datoteke dnevnika pogon za svaki pogon kao što je se pripremite. Datoteke dnevnika pogon pohranjuju se na lokalnom računalu, ne i na pogon sam. Će prijenos datoteke dnevnika prilikom stvaranja posla uvoza. Datoteke dnevnika pogon sadrži ID pogon i BitLocker ključ, kao i druge podatke o pogon.
**Važno**: svakom tvrdom disku pripremite rezultirat će datoteke dnevnika. Kada stvarate posao uvoza pomoću portala za klasični, morate prenijeti sve datoteke dnevnika pogona koje su dio taj zadatak uvoza. Pogoni bez dnevnik datoteka nije obrađen.

8.  Nakon dovršetka disk Priprema mijenjati podatke na pogonima tvrdi disk ili datoteke dnevnika.

U nastavku su naredbe i primjeri za pripremu tvrdi disk pomoću alata za klijent Azure uvoz/izvoz.

Azure uvoz/izvoz klijent PrepImport naredba alat za prvu sesiju Kopiraj da biste kopirali direktorij:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Primjer:**

U primjeru u nastavku ćemo kopirate sve datoteke i sub direktorija iz H:\Video postavljen na X: tvrdi disk. Podaci će se uvesti na račun za pohranu odredište naveden ključ za račun za pohranu i u spremnika za pohranu koji se naziva videozapis. Ako je spremnik za pohranu nema, će biti stvorena. Ta naredba će oblikovati i šifriranje cilj tvrdi disk.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:storageaccountkey /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Azure uvoz/izvoz klijent PrepImport naredba alat za sesije kasnije Kopiraj da biste kopirali direktorij:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Za sesije kasnije Kopiraj isti tvrdi disk, navedite isti naziv datoteke dnevnika i unesite novu sesiju ID; Nema potrebe omogućuje pohranu računa ključ i ciljne pogon ponovno ni da biste formatirali ili šifriranje pogona. U ovom primjeru smo kopirate H:\Photo mapu i njezin direktorija sub iste ciljni pogon, u spremnika za pohranu koji se naziva fotografiju.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Azure uvoz/izvoz klijentskih alata PrepImport naredbu za prvu sesiju Kopiraj da biste kopirali datoteke:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Azure uvoz/izvoz klijentskih alata PrepImport naredbu za sesije kasnije Kopiraj da biste kopirali datoteke:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Zapamti**: podaci će biti uvezeni kao bloka blob-ova. Da biste uvezli podatke kao BLOB-Ova stranica možete koristiti parametar /BlobType. Na primjer, ako uvozite VHD datoteke koje će biti postavljena kao diskova na programa Azure VM, morate ih uvesti kao BLOB-Ova stranica. Ako niste sigurni koji blob biste vrstu odabrali, možete odrediti /blobType:auto i ćemo vam pomoći odrediti ispravnu vrstu. U ovom slučaju sve vhd i vhdx datoteke će biti uvezeni kao BLOB-Ova stranica, a ostatak će biti uvezeni kao bloka blob-ova.

Potražite dodatne informacije o korištenju alata za klijent Azure uvoz/izvoz u [Priprema tvrdi pogoni za uvoz](https://msdn.microsoft.com/library/dn529089.aspx).

Osim toga, pogledajte [Uzorka tijeka rada za pripremu teško pogoni za posao uvoza](https://msdn.microsoft.com/library/dn529097.aspx) detaljnije detaljne upute.  

### <a name="create-the-import-job"></a>Stvorite zadatak uvoza

1.  Kada ste spremni na pogon, pomaknite se na račun za pohranu na [portalu klasičan](https://manage.windowsazure.com) pa prikaz na nadzornoj ploči. U odjeljku **Brzi Glance**, kliknite **Stvori posao uvoza**. Pregled postupka, a zatim odaberite potvrdni okvir da biste naznačili da ste pripremili na pogon i imate dostupne pogon dnevnik datoteku.

2.  U koraku 1, navođenje podataka za kontakt za osoba koja je zadužena za web-mjesto posla uvoza i u okvir za povratnu adresu. Ako želite spremiti podatke iz zapisnika opširno za posao uvoza, potvrdite mogućnost da biste **spremili opširno prijava moj spremnik blob 'waimportexport'**.

3.  U koraku 2, prenesite datoteke dnevnika pogon koje ste nabavili tijekom korak za pripremu pogon. Morat ćete prijenos jedne datoteke za svaki pogon na koji ste pripremili.

    ![Stvorite zadatak uvoza - korak 3](./media/storage-import-export-service/import-job-03.png)

4.  U koraku 3 unesite opisni naziv za posao uvoza. Imajte na umu da se ime koje unesete može sadržavati samo mala slova, brojeve, crtice, a podvlake, morate pokrenuti slovom, a ne smije sadržavati razmake. Koristit će se ime koje ste odabrali za praćenje poslova dok su u tijeku i kad su napravljeni.

    Nakon toga odaberite regiju centar podataka s popisa. Centar za područje podataka označava podatkovnog centra i adresa koji moraju postaviti pakiranju. Potražite u najčešćim Pitanjima ispod dodatne informacije.

5.  U koraku 4, odaberite svoje povrata prijenosni s popisa, a zatim unesite broj računa prijenosni. Microsoft će koristiti taj račun za isporuku pogona natrag nakon dovršetka posla uvoza.

    Ako imate broja za praćenje, na popisu odaberite na prijenosni isporuke i unesite broj za praćenje.

    Ako vam ne ste broja za praćenje, odaberite **mogu će poslati podatke o mojoj dostave za posao uvoza kada se otpremljene Moje paketa**, dovršite postupak uvoza.

6. Unos broja za praćenje nakon otpremljene paket, vratite na stranicu **Uvoz/izvoz** za vaš račun za pohranu na portalu klasični na popisu odaberite svoj posao pa odaberite **Dostavu informacije**. Kretanje kroz čarobnjak, a zatim unesite broj za praćenje u koraku 2.

    Ako je broj za praćenje se neće ažurirati unutar dva tjedna stvaranja posla, zadatak će isteći.

    Ako je zadatak u stanju stvaranje, dostavu ili Transferring, možete ažurirati i prijenosni broj računa vaše u koraku 2 čarobnjaka. Nakon što zadatak u stanju pakiranje, ne možete ažurirati broj računa vaše prijenosni za taj zadatak.

7. Na nadzornoj ploči za portala možete pratiti tijek zadatka. Potražite u članku svakog pojedinog statusa zadatka u prethodnom odjeljku što znači da pregledom [statusa zadatka](#viewing-your-job-status).

## <a name="how-to-create-an-export-job"></a>Kako stvoriti zadatak za izvoz?

Stvorite zadatak programa izvoza obavijestiti da koje će biti uključena jedan ili više prazne pogone podatkovnog centra tako da podatke možete izvesti s računa za pohranu pogone i pogone zatim kupcu koji servis za uvoz/izvoz.

### <a name="prepare-your-drives"></a>Priprema pogonima

Sljedeće provjere prije preporučuje se za pripremanje pogonima na zadatak izvoza:

1. Potvrdite okvir broj diskova obavezno pomoću naredbe PreviewExport alat za Azure uvoz/izvoz. Dodatne informacije potražite u članku [Pregled korištenja pogon za posao za izvoz](https://msdn.microsoft.com/library/azure/dn722414.aspx). Olakšava vam pregled korištenja pogon za blob polja odabrana, ovisno o veličini pogona namjeravate koristiti.

2. Potvrdite da ste možete čitanje/pisanje na tvrdi disk koji će biti isporučeni za zadatak izvoza.

### <a name="create-the-export-job"></a>Stvorite zadatak izvoza

1.  Da biste stvorili zadatak programa izvoza, idite na račun za pohranu na [Klasični portal](https://manage.windowsazure.com)i prikaz na nadzornoj ploči. U odjeljku **Brzi Glance**, kliknite **Stvori zadatak za izvoz** , a zatim nastavite slijediti čarobnjak.

2.  U koraku 2, navođenje podataka za kontakt za osoba koja je zadužena za taj zadatak izvoza. Ako želite spremiti podatke opširno zapisnika za zadatak izvoza, potvrdite mogućnost da biste **spremili opširno prijava moj spremnik blob 'waimportexport'**.

3.  U koraku 3 Odredite koji blob podatke koje želite izvesti s računa za pohranu u prazno pogon ili pogona. Možete odabrati da biste izvezli sve blob podatke na računu za pohranu ili možete odrediti koji blob-ova ili skupova blob-ova za izvoz.

    Da biste odredili blob da biste izvezli, koristite birač **Jednako** , i navedite relativni put do blob-om, počevši od naziv spremnika. Da biste naveli korijenski spremnik pomoću *$root* .

    Da biste odredili počevši s prefiksom sve blob-ova, koristite birač **Započinje s** i navedite prefiks, počevši s kosom crtom '/'. Prefiks možda prefiks naziv spremnika, naziv dovršeno spremnika ili naziv dovršeno spremnika slijedi prefiks blob naziv.

    Sljedeća tablica sadrži primjere valjanih blob putova:

    Birač|Put blobova platforme|Opis
    ---|---|---
    Počinje s|/|Izvozi sve blob-ova u račun za pohranu
    Počinje s|/$root /|Izvozi sve blob-ova u korijenski spremnik
    Počinje s|/Book|Izvozi sve blob-ova u bilo kojem spremniku koji počinje s prefiksom **adresara**
    Počinje s|/Music/|Izvozi sve blob-ova u spremniku **glazbu**
    Počinje s|/ glazbu/ljubav|Izvozi sve blob-ova u spremniku **glazbu** koje započinju s prefiksom **ih**
    Jednako|$root/logo.bmp|Izvozi bloba **logo.bmp** u korijenski spremnik
    Jednako|Videos/story.MP4|Izvozi bloba **story.mp4** u spremniku **videozapisa**

    Morate navesti putova blob valjani oblika da biste izbjegli pogreške tijekom obrade, kao što je prikazano u snimka zaslona.

    ![Stvorite zadatak izvoza - korak 3](./media/storage-import-export-service/export-job-03.png)


4.  U koraku 4, unesite opisni naziv za zadatak izvoza. Ime koje unesete mogu sadržavati samo mala slova, brojeve, crtice, a podvlake, morate pokrenuti slovom, a ne smije sadržavati razmake.

    Centar za područje podataka će vas upozoriti podatkovnom centru koji moraju postaviti pakiranju. Potražite u najčešćim Pitanjima ispod dodatne informacije.

5.  U koraku 5, odaberite svoje povrata prijenosni s popisa pa unesite broj računa prijenosni. Microsoft će koristiti taj račun za isporuku pogonima natrag nakon dovršetka posla izvoz.

    Ako imate broja za praćenje, na popisu odaberite na prijenosni isporuke i unesite broj za praćenje.

    Ako vam ne ste broja za praćenje, odaberite **mogu će poslati podatke o mojoj dostave za izvoz posao kada se otpremljene Moje paketa**, dovršite postupak izvoza.

6. Unos broja za praćenje nakon otpremljene paket, vratite na stranicu **Uvoz/izvoz** za vaš račun za pohranu na portalu klasični na popisu odaberite svoj posao pa odaberite **Dostavu informacije**. Kretanje kroz čarobnjak, a zatim unesite broj za praćenje u koraku 2.

    Ako je broj za praćenje se neće ažurirati unutar dva tjedna stvaranja posla, zadatak će isteći.

    Ako je zadatak u stanju stvaranje, dostavu ili Transferring, možete ažurirati i prijenosni broj računa vaše u koraku 2 čarobnjaka. Nakon što zadatak u stanju pakiranje, ne možete ažurirati broj računa vaše prijenosni za taj zadatak.

    > [AZURE.NOTE] Ako je blob-om za moguće izvesti koristi prilikom kopiranja na tvrdom disku, servis za uvoz/izvoz Azure će stvorite snimku blob-om i kopirajte snimke.

7.  Na nadzornoj ploči na portalu klasični možete pratiti tijek zadatka. U odjeljku svakog pojedinog statusa posla što znači da u prethodnom odjeljku na "pregledavanje statusa zadatka".

8.  Kada primite pogone s izvezene podatke, možete pregledavati i kopirajte tipki BitLocker generira servisa na disk. Idite na račun za pohranu na portalu klasični te kliknite karticu Uvoz/izvoz. Na popisu odaberite svoj posao izvoz, a zatim kliknite gumb prikaz tipki. Tipke BitLocker prikazivati kao što je prikazano u nastavku:

    ![Prikaz ključevima programa BitLocker za zadatak izvoza](.\media\storage-import-export-service\export-job-bitlocker-keys.png)

Idite do u odjeljku najčešća Pitanja ispod kao pokriva najčešćih pitanja kupaca naići prilikom korištenja ove usluge.

## <a name="frequently-asked-questions"></a>Najčešća pitanja ##


**Koliko će trajati da biste kopirali podatke nakon Moje (diskovima) dosegne podatkovnog centra?**

Vrijeme da biste kopirali razlikuje se ovisno o različitim čimbenike kao što su vrsta posla, vrsta i veličina podataka koja se kopira veličina diskova navedena i postojeće radno opterećenje. To može se razlikovati od nekoliko dana za nekoliko tjedana, ovisno o sljedećih čimbenika. Stoga je teško pružaju Općenito procjenu. Servis za pozive pokušava uspostaviti optimiziranje posla tako da kopirate više pogona paralelno kada je to moguće. Ako imate vremena ključnih uvoz/izvoz posao, stupili nam za procjenu.

**Kada koristiti servis za uvoz/izvoz Azure?**
Jedan razmotrite pomoću izvoza uvoz Azure ako prijenos ili preuzimanje putem mreže traje otprilike kao procjenu više od sedam dana. Možete izračunati koliko će trajati pomoću bilo kojeg online kalkulatora ili [Preuzimanje](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/archive/master.zip) onaj koji se nalazi u Azure uvoz izvoz REST API oglednim u spremištu Azure uzoraka pri [Kalkulatora brzinu prijenosa podataka](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html). Ovo nije točan izračuna, ali samo grubi oznaka.

**Mogu li koristiti servis za uvoz/izvoz Azure s računom Voditelj resursa za pohranu?**

Ne, nije moguće kopiranje podataka ili s računa za pohranu resursima izravno putem servisa Azure uvoz/izvoz. Servis podržava samo računi klasični prostora za pohranu. Voditelj resursa za pohranu računa podrška će uskoro dostupno. Kao alternativu, možete uvesti podatke u račun za klasični prostora za pohranu i migrirati s računom za pohranu resursima pomoću [AzCopy](storage-use-azcopy.md) ili CopyBlob.

**Mogu li kopirati Azure datoteka pomoću servisa Azure uvoz/izvoz?**

Ne, servis za uvoz/izvoz Azure podržava samo bloka blob-ova i blob-Ova stranica. Sve ostale prostora za pohranu vrste uključujući Azure datoteke, tablice i redova nisu podržani.

**Servis za uvoz/izvoz Azure dostupna je za CSP pretplate?**

Ne, uvoz/izvoz Azure usluga ne podržava CSP pretplate. Podrška će biti dodan u budućnosti.

**Možete li preskočiti korak pripreme pogon za posao uvoza ili možete li pripremiti pogon bez kopiranja?**

Bilo koji pogon na koji želite isporuka za uvoz podataka potrebno pripremiti pomoću alata za klijent Azure uvoz/izvoz. Da biste kopirali podatke na pogon morate koristiti alat za klijenta.

**Potrebne za izvođenje bilo koji disk Priprema prilikom stvaranja zadatak programa izvoza?**

Preporučuje se nema, ali neke prije provjere. Potvrdite okvir broj diskova obavezno pomoću naredbe PreviewExport alat za Azure uvoz/izvoz. Dodatne informacije potražite u članku [Pregled korištenja pogon za posao za izvoz](https://msdn.microsoft.com/library/azure/dn722414.aspx). Olakšava vam pregled korištenja pogon za blob polja odabrana, ovisno o veličini pogona namjeravate koristiti. Provjeriti čitanje i pisanje na tvrdi disk koji će biti isporučeni za zadatak izvoza.

**Što se događa ako slučajno slanje podizanje tvrdog diska koji nisu u skladu za podržane preduvjeti?**

Azure podatkovnog centra u će vratiti na pogon koji nisu u skladu podržani zahtjevima vama. Ako samo neke pogone u paketu zadovoljava preduvjete za podršku, obradit će se one pogona i pogone koji ne zadovoljava preduvjete vratit će se na vas.

**Možete otkazati Moj posao?**

Možete otkazati posao ima li status pri stvaranju ili dostavu.

**Koliko dugo Prikaz statusa dovršenih zadataka na portalu klasični?**

Možete pogledati status dovršene poslove do 90 dana. Dovršene poslove izbrisat će se nakon 90 dana.

**Ako želite li uvoz i izvoz više od 10 pogona, što učiniti?**

Jedan uvoz ili izvoz posao možete referencirati samo 10 pogonima u jedan zadatak za servis za uvoz/izvoz. Ako želite isporuka više od 10 pogona, možete stvoriti više zadataka. Pogona koje su vezane uz isti posao mora poslane zajedno u istom paket.

**Mogu li koristiti USB SATA prilagodnik koji nije na popisu preporučene?**

Ako imate pretvornika koji nije naveden, pokušajte pokretanja alata za uvoz/izvoz Azure pomoću svoje pretvornika za pripremu pogon i potražite u članku funkcionira prije kupnje pretvornika podržane.

**Ne servis oblikovati pogona prije vraćanja ih?**

ne. Svi pogoni šifrirane s BitLocker.

**Možete li kupiti pogona za uvoz/izvoz poslove od Microsofta?**

ne. Morat ćete isporuka vlastite pogona za oba uvoz i izvoz zadatke.

**Nakon dovršetka posla uvoza, što će moje podatke izgledaju na računu za pohranu? Bit će Moje hijerarhiji direktorija zadržan?**

Prilikom pripreme tvrdi disk za posao uvoza, odredište određen u /dstdir: parametar. Ovo je spremnik odredište u račun za pohranu kopirati podatke iz tvrdi disk. U ovom spremniku odredište virtualne direktorije stvaraju za mape s tvrdog diska, a blob polja stvaraju se za datoteke.

**Ako je pogon datoteke koje već postoje u moj račun za pohranu, servis prebrisat će postojeće blob-ova u moj račun za pohranu?**

Prilikom pripreme pogon, možete odrediti hoće li odredišna datoteka mora biti prebrisati ili zanemarene pomoću parametar pod nazivom /Disposition: < Preimenovanje | bez prebrisati | prebrisati >. Prema zadanim postavkama, servis će preimenovati nove datoteke umjesto Prebriši postojeće blob-ova.

**Alat za klijent Azure uvoz/izvoz kompatibilan je s 32-bitne operacijske sustave?**
ne. Alat za klijenta samo kompatibilan je s operacijski sustavi za 64-bitni Windows. Pogledajte odjeljak operacijski sustavi u [stara requisites](#pre-requisites) popis podržanih verzija OS-a.

**Treba li uključiti bilo što drugo osim tvrdi disk u moje paketa?**

Provjerite isporuka samo tvrdog diska. Sadrže stavke kao što su power opskrbu ili USB kabela.

**Imati za isporuku Moje pogona pomoću FedEx ili DHL?**

Možete isporuka pogona podatkovnog centra pomoću bilo koje poznate prijenosni kao što su FedEx, DHL, UPS ili NAM servisa poštanski. Međutim, za dostavu pogona natrag iz podatkovnog centra, navedite broj računa FedEx u SAD-a i EU-a ili broj računa DHL u Azije i Australija regijama.

**Postoje li ograničenja s prikazanom adresom za isporuku pogon Međunarodno?**

Uzmite u obzir fizičke medijske sadržaje koje ste isporučuju morati Unakrsna međunarodne obrubi. Vi ste odgovorni za osiguravanje da fizičke medijske sadržaje i podaci su uvesti i/ili izvesti u skladu s primjenjivih zakona. Prije dostavu fizička medijskih sadržaja, potvrdite okvir uz vaše Savjetnici da biste potvrdili da medijskih sadržaja i podataka možete legalno otpremljene identificirani podatkovnog centra u. To će pomoći da biste bili sigurni da je dostigne Microsoft pravovremeno.

**Prilikom stvaranja posao adresu dostave je na mjesto koje se razlikuje od svoju lokaciju račun za pohranu. Što da radim?**

Neki račun mjesta za pohranu mapiraju se na zamjensku dostave mjesta. Prethodno mjesta dostupnih dostave mogu privremeno mapirati na druga mjesta. Uvijek provjeri dostave adresu koja je navedena tijekom stvaranja posla prije dostavu pogonima.

**Zašto se Moj posao status u na klasični portala izgovorite "dostavu" kada prijenosni web-mjesta prikazuje paket za Moje je isporučen?**

Status na portalu klasični mijenja se iz dostave za prijenos kada pogon obrade pokreće. Ako na disk je Pristigla funkciju, ali je počeo obrade, statusa zadatka prikazat će se kao dostave.

**Kada dostavu Moje pogon na prijenosni traži ime kontakta za centar za podatke i telefonski broj. Što mora sadržavati?**

Telefonski broj na navedeni su vam tijekom stvaranja posla. Ako vam je potrebna ime kontakta, obratite nam se na waimportexport@microsoft.com i možemo će vam ponuditi te podatke.

**Mogu li koristiti servisa Azure uvoz/izvoz da biste kopirali O365 poštanskih sandučića u pst datoteke i podataka iz sustava SharePoint?**

Pogledajte [Uvoz PST datoteka ili podataka sustava SharePoint u Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Mogu li koristiti servisa Azure uvoz/izvoz da biste kopirali kopija izvanmrežno sigurnosne kopije servisa Azure?**

Pogledajte [tijek rada izvanmrežno sigurnosnu kopiju u Azure sigurnosnu kopiju](../backup/backup-azure-backup-import-export.md).

## <a name="see-also"></a>Vidi također:

- [Postavljanje Azure uvoz/izvoz alata za klijenta](https://msdn.microsoft.com/library/dn529112.aspx)

- [Prijenos podataka s pomoćnog programa naredbenog retka AzCopy](storage-use-azcopy.md)

- [Azure uvoz izvoz REST API uzorka](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)
