<properties
    pageTitle="Šifriranje servisa Azure prostora za pohranu za podatke na ostale | Microsoft Azure"
    description="Šifriranje Azure blobova na strani servisa kada spremanje podataka pomoću značajke šifriranja servis za pohranu za Azure i dešifriranje prilikom dohvaćanja podataka."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="robinsh"/>

# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Šifriranje servisa Azure prostora za pohranu za podatke na ostale

Azure prostora za pohranu servisa šifriranja (SSE) za podatke na ostale pridonosi zaštiti i zaštitili vaše podataka da bi zadovoljili vaše tvrtke ili ustanove sigurnost i usklađenost p. S tom značajkom Azure prostora za pohranu automatski šifrira podataka prije održavanju za pohranu i dešifrira prije dohvaćanja. Šifriranje, dešifriranje i upravljanja ključem su svi prozirne korisnicima.

Sljedeći odjeljci sadrže detaljne upute za korištenje značajke šifriranja servis za pohranu kao i podržani scenariji i korisničkog sučelja.

## <a name="overview"></a>Pregled

Azure prostora za pohranu daje opsežan skup sigurnosne mogućnosti koje zajedno omogućiti inženjerima omogućuje stvaranje aplikacije za sigurnu. Podatke možete zaštićenim na putu između aplikacije i Azure pomoću [Klijentsko šifriranja](storage-client-side-encryption.md), HTTPs ili SMB 3.0. Šifriranje servis za pohranu nudi šifriranje na ostale, rukovanje šifriranja, dešifriranje i upravljanja ključem u cijelosti prozirne način. Svi podaci je šifriran pomoću 256-bitno [AES šifriranje](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednu od Najveća bloka ciphers dostupna.

SSE radi šifrira podatke kada napisan Azure prostora za pohranu i može se koristiti za blokiranje blob-ova, blob-Ova stranica i dodavanje blob-ova. Radi sljedeće:

-   Opće namjene prostora za pohranu računi i računi servisa spremište blobova platforme
-   Standardni prostora za pohranu i spremište Premium 
-   Sve zalihosti razine (LRS, ZRS, GRS, RA GRS)
-   Azure Voditelj resursa za pohranu računi (ali ne klasične) 
-   Sve regije

Da biste omogućili ili onemogućili šifriranje servis za pohranu za račun za pohranu, prijavite se na [portal za Azure](https://azure.portal.com) , a zatim odaberite račun za pohranu. Na plohu postavke potražite u odjeljku Blob servisa kao što je prikazano u snimka zaslona, a zatim kliknite šifriranje.

![Mogućnost šifriranja portala mogućnosti ispis](./media/storage-service-encryption/image1.png)

Nakon što kliknete postavku šifriranja, možete je omogućiti ili onemogućiti šifriranje servis za pohranu.

![Svojstva portala snimka zaslona s prikazom šifriranje](./media/storage-service-encryption/image2.png)

##<a name="encryption-scenarios"></a>Scenariji za šifriranje

Šifriranje servis za pohranu može se omogućiti na razini račun za pohranu. Podržava sljedeće scenarije kupca:

-   Šifriranje bloka blob-ova, dodavanje blob-ova i blob-Ova stranica.

-   Šifriranje arhivirane VHDs i predloške lokalnim ne unese za Azure.

-   Šifriranje podlozi diskova OS i podaci za IaaS VMs stvoren pomoću svoje VHDs.

SSE ima sljedeća ograničenja:

-   Šifriranje klasični prostora za pohranu računa nije podržana.

-   Šifriranje klasični prostora za pohranu računi migrira se na Voditelj resursa za pohranu računa nije podržana.

-   Kada je omogućen šifriranje postojećih podataka – SSE samo šifrira novostvorenu podataka. Ako, primjerice stvaranje novog računa Voditelj resursa za pohranu, ali ne uključite šifriranja, a zatim prijenos blob-ova ili arhivirane VHDs taj račun za pohranu, a zatim uključite SSE, te blob-ova neće biti šifrirane osim ako je potrebno ponovno napisati ili kopirati.

-   Podrška za tržište - Omogući šifriranje VMs stvorene iz trgovine pomoću [portala za Azure](https://portal.azure.com), PowerShell i Azure EŽA. Slika osnovni VHD će ostati šifrirana; Međutim, sve zapisivanja gotovo kada je na VM spun prema gore bit će šifrirana.

-   Tablica, redove, a neće biti šifriran podatkovne datoteke.

##<a name="getting-started"></a>Početak rada

###<a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Korak 1: [Stvaranje novog računa za pohranu](storage-create-storage-account.md).

###<a name="step-2-enable-encryption"></a>Korak 2: Omogućavanje šifriranje.

Možete omogućiti šifriranje pomoću [portala za Azure](https://portal.azure.com).

> [AZURE.NOTE] Želite li programatski Omogućivanje i onemogućivanje šifriranje servis za pohranu na račun za pohranu, možete koristiti [Azure prostora za pohranu resursa davatelja REST API -JA](https://msdn.microsoft.com/library/azure/mt163683.aspx), [Biblioteka davatelj usluga za pohranu resursa klijenta za .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShell](../powershell-install-configure.md)ili [Azure EŽA](storage-azure-cli.md).

###<a name="step-3-copy-data-to-storage-account"></a>Korak 3: Kopiranje podataka na račun za pohranu

Ako omogućite SSE na račun za pohranu, a zatim napisati blob-ova za taj račun za pohranu, u BLOB-Ova će šifrirana. Blob-ova već nalazi u taj račun za pohranu neće biti šifriran dok se ne mogu se potrebno ponovno napisati. Možete kopirati podatke s računa za jednu za pohranu na jedan s SSE šifrirane, ili čak i omogućiti SSE i kopirajte s blob-ova iz jednog spremnika u drugu u li prethodne podatke šifriran. Da biste to postigli možete koristiti bilo koji od sljedećih alata.

#### <a name="using-azcopy"></a>Korištenje AzCopy

AzCopy je Windows naredbenog retka utility namijenjen kopiranje podataka da biste i iz spremišta blobova platforme Microsoft Azure, datoteke i tablici pomoću jednostavne naredbe optimalne performanse. To možete koristiti da biste kopirali vaše blob polja s jednog računa za pohranu neku drugu koja ima SSE omogućena. 

Da biste saznali više, posjetite [prenijeti podatke s AzCopy naredbenog retka uslužni](storage-use-azcopy.md).

#### <a name="using-the-storage-client-libraries"></a>Pomoću biblioteka za pohranu klijenta

Možete kopirati podatke blobova platforme za i iz spremišta blobova ili između prostora za pohranu računa pomoću naš bogatog skupa prostora za pohranu klijenta biblioteke uključujući .NET, C++, jezika Java, Android, Node.js, PHP, Python i Ruby.

Da biste saznali više, posjetite naš [Početak rada s Azure blobova pomoću .NET](storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Pomoću programa Explorer prostora za pohranu

Prostor za pohranu explorer možete koristiti za stvaranje računa za pohranu prijenos i preuzimanje podataka, pregledati sadržaj blob-ova i kretanje kroz direktorija. Nešto od sljedećeg možete koristiti da biste prenijeli blob polja na račun servisa za pohranu šifriranjem omogućen. S nekim prostora za pohranu programu Software Explorer koje možete kopirati i podataka iz postojeće blobova u drugi spremnik u račun za pohranu ili na novi račun za pohranu koji sadrži SSE omogućena.

Da biste saznali više, posjetite [Azure prostora za pohranu programu Software Explorer](storage-explorers.md).

###<a name="step-4-query-the-status-of-the-encrypted-data"></a>Korak 4: Upit status šifriranih podataka

Ažuriranu verziju biblioteka za pohranu klijenta implementiran koji vam omogućuje da upite stanju objekt da biste odredili ako je šifriran ili ne. Primjeri dodat će se na ovaj dokument u skorijoj budućnosti.

S aplikacijom, možete nazvati [Svojstva računa za početak](https://msdn.microsoft.com/library/azure/mt163553.aspx) provjerite ima li račun za pohranu šifriranje omogućeno ili prikaz svojstava račun za pohranu na portalu za Azure.

##<a name="encryption-and-decryption-workflow"></a>Šifriranje i dešifriranje tijeka rada

Ovdje je kratak opis tijeka rada za šifriranje/dešifriranje:

-   Klijent omogućuje šifriranje na računu za pohranu.

-   Kada je korisnik odabrao piše nove podatke (STAVITI Blob STAVITI blok, STAVITE stranice, itd.) sa spremištem blobova; svaki unos je šifriran pomoću 256-bitno [AES šifriranje](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednu od Najveća bloka ciphers dostupna.

-   Kada je korisnik odabrao treba za pristup podacima (DOHVATI Blob, itd.), podaci automatski dešifriranu prije vraćanja korisnika.

-   Ako je onemogućen šifriranja, novi zapisivanja više nisu šifrirane i postojeće šifrirane podaci ostaju šifrirane dok je potrebno ponovno napisati korisnik. Dok je omogućen šifriranja, upisivanje blobova bit će šifrirana. Stanje podataka ne mijenja se s korisnikom prebacivanja između omogućivanja i onemogućivanja šifriranja za račun za pohranu.

-   Sve ključeva za šifriranje su pohranjeni, šifrirane i upravlja Microsoft.

##<a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Najčešća pitanja o šifriranje servis za pohranu za podatke na ostale

**P: imaju postojeći račun klasični prostora za pohranu. Možete omogućiti SSE na njoj?**

A: ne, SSE podržano je samo na računima Voditelj resursa za pohranu.

**P: kako šifrirati podatke u moj račun klasični prostora za pohranu?**

A: možete stvoriti novi račun Voditelj resursa za pohranu i kopirajte podatke pomoću [AzCopy](storage-use-azcopy.md) s postojećeg računa za klasični prostora za pohranu na račun za pohranu novostvorenu Voditelj resursa. 

Druga mogućnost je da biste migrirali računa klasični prostora za pohranu na račun za upravljanje resursa za pohranu. Dodatne informacije potražite u članku [Platformu podržani od IaaS s resursima za migraciju od klasičnog upravitelju resursa](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/).

**P: imaju postojeći račun Voditelj resursa za pohranu. Možete omogućiti SSE na njoj?**

A: da, ali samo upravo napisali blob-Ova će biti šifrirana. To ne vratite i šifriranje podatke koji su već postoji. 

**P: aspekt za šifriranje trenutne podatke u postojeći račun Voditelj resursa za pohranu?**

A: možete omogućiti SSE u bilo kojem trenutku u računu Voditelj resursa za pohranu. Međutim, blob-ova koji su već postoji neće biti šifriran. Da biste šifrirali te blob-ova, možete ih kopirati na neki drugi naziv ili drugi spremnik, a zatim uklonite šifrirane verzije.

**P: koristim prostora za pohranu Premium; mogu li koristiti SSE?**

A: da, SSE podržano je na standardni prostora za pohranu i spremište Premium.

**P: Ako se Stvori novi račun za pohranu i omogućiti SSE, a zatim stvorite novi VM računa za pohranu, znači da je šifriran Moje VM?**

O: da. Bilo koji diskova stvorili koje koriste novi račun za pohranu bit će šifrirana, pod uvjetom da se stvaraju kada je omogućen SSE. Ako na VM stvoren pomoću Azure tržište, slika osnovni VHD će ostati šifrirana; Međutim, sve zapisivanja gotovo kada je na VM spun prema gore bit će šifrirana.

**P: je li moguće stvoriti nove račune za pohranu s SSE omogućeno korištenje Azure PowerShell i EŽA Azure?**

O: da.

**P: koliko više prostora za pohranu Azure stoji na ako je omogućeno SSE?**

A: postoji bez dodatnih troškova.

**P: tko upravlja ključeve za šifriranje?**

A: tipki upravlja Microsoft.

**P: mogu li koristiti vlastitu ključeva za šifriranje?**

A: Radimo na pruža mogućnosti za korisnike možete prenijeti vlastite ključeva za šifriranje.

**P: mogu li opozvati pristup ključeva za šifriranje?**

O: Zasad ne; tipke potpuno upravlja Microsoft.

**P: je SSE po zadanom omogućena prilikom stvaranja novog računa za pohranu?**

A: SSE nije omogućen po zadanom. pomoću portala za Azure je omogućiti. Programski i možete omogućiti tu značajku pomoću prostora za pohranu resursa davatelja REST API-JA.

**P: kako se to razlikuje od šifriranje pogona Azure?**

A: značajka se koristi za šifriranje podataka u spremište blobova platforme Azure. Šifriranje diska Azure koristi se za šifriranje s operacijskim Sustavom i podataka diskova IaaS VMs. Više detalja, posjetite naš [Vodič za sigurnost prostora za pohranu](storage-security-guide.md).

**P: što se događa ako Omogućivanje SSE, i zatim pregledate i omogućite šifriranje Azure na na diskova?**

A: to će radite neometano. Podaci će biti šifrirane na oba načina.

**P: računa za pohranu je postaviti tako da je replicirati zemlj.-redundantly. Ako je omogućiti SSE, bit će kopiju redundantnih i šifrirana?**

A: da, šifrirane sve kopije računa za pohranu i sve mogućnosti zalihosti – lokalno suvišne prostora za pohranu (LRS), Zone Redundant prostora za pohranu (ZRS), zemlj Redundant prostora za pohranu (GRS) i pristup čitanju zemlj Redundant prostora za pohranu (RA GRS) – podržane.

**P: ne možete omogućiti šifriranje na moj račun za pohranu.**

A: je račun Voditelj resursa za pohranu? Klasični prostora za pohranu računi nisu podržani. 

**P: SSE dopušteno je samo na određenim područjima?**

O: SSE dostupna je u svim regijama. 

**P: kako se obratiti netko ako imam problema ili želite povratne informacije?**

A: obratite [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com) probleme vezane uz šifriranje servis za pohranu.

##<a name="next-steps"></a>Daljnji koraci

Azure prostora za pohranu nudi opsežan skup sigurnosne mogućnosti koji zajedno omogućiti inženjerima omogućuje stvaranje sigurnog aplikacije. Dodatne informacije potražite u članku [Vodič za sigurnost prostora za pohranu](storage-security-guide.md).
