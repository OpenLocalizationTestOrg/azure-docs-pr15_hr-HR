<properties
    pageTitle="Otklanjanje problema s prostora za pohranu datoteka Azure | Microsoft Azure"
    description="Otklanjanje problema s prostora za pohranu datoteka Azure"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genli"/>

# <a name="troubleshooting-azure-file-storage-problems"></a>Otklanjanje poteškoća za pohranu datoteka Azure

U ovom se članku navedene najčešće probleme vezane uz Microsoft Azure pohrani kada se povežete s klijentima za Windows i Linux. Također nudi mogući uzroci i rješenja za te probleme.

**Opći problemi (koja se pojavljuju u klijentskim programima sustava Windows i Linux)**

- [Kvota pogreška prilikom otvaranja datoteke](#quotaerror)

- [Slabe performanse prilikom pristupa Azure pohrani iz sustava Windows ili Linux](#slowboth)

**Poteškoće klijenta za Windows**

- [Slabe performanse prilikom pristupa za pohranu datoteka uz Azure Windows 8.1 ili Windows Server 2012 R2](#windowsslow)

- [Pogreška 53 pokušaja postavljanja zajedničko korištenje datoteka programa Azure](#error53)

- [Neto koristi je uspjela, ali ne vidim datoteku Azure zajedničko korištenje postavljene u programu Windows Explorer](#netuse)

- [Moj račun za pohranu sadrži "/" i neto koristiti naredbu ne uspije](#slashfails)

- [Moje aplikacije/usluga ne može pristupiti postavljenim Azure datoteke.](#accessfiledrive)

- [Dodatne preporuke za optimizaciju performansi](#additional)

**Problemi Linux klijenta**

- [Pogreška "Kopirate datoteke na odredište koje ne podržava šifriranje" kada prijenos/kopiranje datoteka Azure datoteke](#encryption)

- [Dijeli "Glavno računalo nije dostupno" Pogreška na postojeću datoteku ili ljuske blokira pri izvođenju popis naredbi na točku postavljanja](#errorhold)

- [Pogreška postavljanja 115 prilikom postavljanja Azure datoteke na Linux VM](#error15)

- [Linux VM pojave slučajni kašnjenja u naredbama kao što su "ls"](#delayproblem)

## <a name="general-problems"></a>Opći problemi
<a id="quotaerror"></a>
### <a name="quota-error-when-trying-to-open-a-file"></a>Kvota pogreška prilikom otvaranja datoteke

U sustavu Windows, primit ćete poruku o pogrešci da otprilike ovako:

**1816 ERROR_NOT_ENOUGH_QUOTA <> – 0xc0000044**

**STATUS_QUOTA_EXCEEDED**

**Nema dovoljno kvote da bi se obradila ta naredba**

**Pokazivač nije valjan vrijednost Preuzmi zadnju pogrešku: 53**

Na Linux, primit ćete poruku o pogrešci da otprilike ovako:

**<filename>[dozvola zabranjen]**

**Premašena je kvota na disku**

#### <a name="cause"></a>Uzrok

Problem se pojavljuje jer ste dostigli gornju granicu Istodobni ručice za otvaranje dopušteni datoteke.

#### <a name="solution"></a>Rješenja

Smanjite broj Istodobni ručice za otvaranje zatvaranjem neke ručice i ponovno pokušajte. Dodatne informacije potražite u članku [Microsoft Azure prostora za pohranu performanse i skalabilnost kontrolnog popisa](storage-performance-checklist.md).

<a id="slowboth"></a>
### <a name="slow-performance-when-accessing-file-storage-from-windows-or-linux"></a>Slabe performanse prilikom pristupanja za pohranu datoteka iz sustava Windows ili Linux

- Ako nemate određene minimalni preduvjet veličina/i, preporučujemo da koristite 1 MB/i veličine postigli optimalne performanse.

- Ako znate da konačna veličine datoteke koje su proširivanje s zapisivanje, a softvera nema problemi s kompatibilnošću prilikom nije još pisane krakom na datoteku koja sadrži nule, zatim postavite veličinu datoteke unaprijed umjesto svaki unos koji se extending unos.

## <a name="windows-client-problems"></a>Poteškoće klijenta za Windows
<a id="windowsslow"></a>
### <a name="slow-performance-when-accessing-the-file-storage-from-windows-81-or-windows-server-2012-r2"></a>Slabe performanse prilikom pristupanja za pohranu datoteka iz Windows 8.1 ili Windows Server 2012 R2

Za klijente koji rade s programom Windows 8.1 ili Windows Server 2012 R2, provjerite je li instaliran hitni popravak [KB3114025](https://support.microsoft.com/kb/3114025) . Hitni popravak poboljšava Stvori i zatvorite ručicu performansi.

Možete pokrenuti sljedeću skriptu da biste provjerili je li hitni popravak instaliran na:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Ako je instaliran hitni popravak, prikazat će se sljedeći rezultat:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies**

**{96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1**

> [AZURE.NOTE]  Windows Server 2012 R2 slike u trgovine Windows Azure imati hitni popravak KB3114025 instalirao zadani Početni u prosincu 2015.

<a id="additional"></a>
### <a name="additional-recommendations-to-optimize-performance"></a>Dodatne preporuke za optimizaciju performansi

Nikad ne stvorite ili otvorite datoteku za predmemorirani/i traži pristup za pisanje, ali ne pristup za čitanje. To jest, kada ste poziv **CreateFile()**, nikad ne odredite samo **GENERIC_WRITE**, ali uvijek navesti **GENERIC_READ | GENERIC_WRITE**. Bolji samo za pisanje nije moguće predmemoriju small zapisivanja lokalno, čak i kad je ručicu samo otvorene datoteke. To nameće loši performanse kazna na small zapisivanja. Imajte na umu da "u" način da biste CRT **fopen()** otvara bolji samo za pisanje.

<a id="error53"></a>
### <a name="error-53-when-you-try-to-mount-or-unmount-an-azure-file-share"></a>"Pogreška 53" kada pokušate učitati ili skidanje zajedničko korištenje datoteka programa Azure

Taj se problem može uzrokovati sljedeći uvjeti:

#### <a name="cause-1"></a>Uzrok 1

"Sustava 53 pogreška. Pristup odbijen." Sigurnosnih vam razloga veze za zajedničko korištenje datoteka Azure su blokirane ako nije šifriranih komunikacijskog kanala i pokušaj veze ne unijeli centra za iste podatke na kojem se nalaze zajedničke datoteke Azure. Šifriranje kanala komunikacije nisu navedeni su ako korisnika klijenta s operacijskim Sustavom ne podržava šifriranje SMB. To je označena je "sustava 53 pogreška. Pristup je zabranjen"poruka o pogrešci kada se korisnik pokuša dostupnosti zajedničko korištenje datoteka s lokalnog ili iz centra za različite podatke. Windows 8, Windows Server 2012 i novijim verzijama svakog zahtjeva Pregovaraj koja obuhvaća 3.0 SMB koji podržava šifriranje.

#### <a name="solution-for-cause-1"></a>Rješenje zbog nekog razloga 1

Povezivanje s klijenta koji zadovoljava preduvjete sustava Windows 8, Windows Server 2012 ili novije verzije ili koji se povezati s virtualnog računala koja se nalazi na istom podatkovnog centra kao račun za Azure prostora za pohranu koji se koristi za zajedničko korištenje datoteka Azure.

#### <a name="cause-2"></a>Uzrok 2

"Pogreška sustava 53" kada postavljate Azure zajedničkom može dogoditi ako je blokirana priključak 445 izlaznog komunikacije Azure datoteke podatkovnog centra. Kliknite [ovdje](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx) da biste vidjeli sažetak koji omogućili ili onemogućili pristup s priključak 445.

Comcast i neke tvrtke ili ustanove IT blokirati priključak. Da biste shvatili je li to razlog iza "sustava 53" poruka, možete koristiti Portqry upita za krajnju točku TCP:445. Ako krajnju točku TCP:445 prikazat će se kao filtrirane TCP priključak blokiran. Ovo je ogledni upit:

`g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Ako TCP 445 blokira pravilo putu mreže, prikazat će se sljedeći rezultat:

**TCP priključak 445 (servis microsoft ds): FILTRIRANJA**

Dodatne informacije o korištenju Portqry potražite u članku [Opis pomoćnog programa naredbenog retka Portqry.exe](https://support.microsoft.com/kb/310099).

#### <a name="solution-for-cause-2"></a>Rješenje zbog nekog razloga 2

Rad s IT tvrtke ili ustanove da biste otvorili priključak 445 odlazni za [Azure IP raspone](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="cause-3"></a>Uzrok 3

"Pogreška sustava 53" i moguće primiti ako komunikacije NTLMv1 je omogućena na klijentskom računalu. Imate NTLMv1 omogućeno stvara manje sigurne klijent. Stoga komunikacije bit će blokirane za Azure datoteke. Da biste provjerili je li to uzrok pogreške, provjerite je li vrijednost 3 postavljen sljedeći potključ registra:

HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel.

Dodatne informacije potražite u temi [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) na mreži TechNet.

#### <a name="solution-for-cause-3"></a>Rješenje zbog nekog razloga 3

Da biste riješili taj problem, vratite vrijednost LmCompatibilityLevel u ključu registra HKLM\SYSTEM\CurrentControlSet\Control\Lsa na zadanu vrijednost od 3.

Azure datoteke podržava samo provjera autentičnosti NTLMv2. Provjerite je li klijente se primjenjuje pravilnik grupe. To će onemogućiti tu pogrešku. I to smatra se sigurnost preporučenim načinom rada. Dodatne informacije potražite u članku [upute za konfiguriranje klijentima omogućuje korištenje NTLMv2 pomoću pravila grupe](https://technet.microsoft.com/library/jj852207(v=ws.11).aspx)

Je postavka preporučenih pravila **samo NTLMv2 poslati odgovor**. To odgovara registra vrijednost 3. Klijenti koristiti samo provjera autentičnosti NTLMv2 i koristiti osiguranje sesije NTLMv2 ako to podržava poslužitelj. Kontrolera domena prihvaćaju LM, NTLM i NTLMv2 provjere autentičnosti.

<a id="netuse"></a>
### <a name="net-use-was-successful-but-dont-see-the-azure-file-share-mounted-in-windows-explorer"></a>Neto koristi je uspjela, ali ne vidite Azure datoteka za zajedničko korištenje postavljene u programu Windows Explorer

#### <a name="cause"></a>Uzrok

Prema zadanim postavkama programa Windows Explorer ne Pokreni kao Administrator. Ako pokrenete **neto koristi** s administratorski naredbeni redak, mapiranje mrežni disk "Kao Administrator." Budući da su mapirani pogoni korisnika usmjereni, korisnički račun koji nije prijavljen ni ne prikazuje pogone ako oni su postavljeni na drugi račun.

#### <a name="solution"></a>Rješenja

Postavljanje zajedničko korištenje putem naredbenog retka koji nisu administratori. Osim toga, možete pratiti [u ovoj se temi TechNet](https://technet.microsoft.com/library/ee844140.aspx) da biste konfigurirali **EnableLinkedConnections** vrijednost registra.

<a id="slashfails"></a>
### <a name="my-storage-account-contains--and-the-net-use-command-fails"></a>Moj račun za pohranu sadrži "/" i neto koristiti naredbu ne uspije

#### <a name="cause"></a>Uzrok

Naredbe **net use** izvršavanja u odjeljku naredbeni redak (cmd.exe) je raščlaniti dodavanjem "/" kao mogućnost za naredbenog retka. Zbog toga pogon mapiranje neće uspjeti.

#### <a name="solution"></a>Rješenja

Da biste riješili problem možete koristiti neku od sljedećih koraka:

• Koristite sljedeću naredbu komponente PowerShell:

`New-SmbMapping -LocalPath y: -RemotePath \\server\share  -UserName acountName -Password "password can contain / and \ etc"`

Iz datoteke grupe za to možete učiniti kao

`Echo new-smbMapping ... | powershell -command –`

• Staviti dvostruke navodnike oko ključ da biste riješili taj problem, osim ako "/" je prvi znak. Ako je, koristite interaktivni način rada i zasebno unesite lozinku ili Obnovi ključeva za ključ koji ne započinju znakom kosa crta (/).

<a id="accessfiledrive"></a>
### <a name="my-applicationservice-cannot-access-mounted-azure-files-drive"></a>Moje aplikacije/usluga ne može pristupiti postavljenim Azure datoteke

#### <a name="cause"></a>Uzrok

Pogona su postavljena po korisniku. Ako program ili servis pokrenut na drugi račun, korisnici neće vidjeti pogon.

#### <a name="solution"></a>Rješenja

Postavljanje pogon iz isti korisnički račun na kojem je li aplikacija. To možete učiniti pomoću alata kao što je psexec.

Umjesto toga stvorite novog korisnika koji sadrži iste ovlasti kao mreža račun servisa ili sustava, a zatim pokrenite **cmdkey** i **neto koristi** u odjeljku taj račun. Korisničko ime mora biti naziv računa za pohranu i lozinka mora biti ključ za račun za pohranu. Druga mogućnost za **Mrežna** je prenesite naziv računa za pohranu i unesite korisničko ime i lozinku za parametre naredbe **net use** .

Nakon što napravite ove upute, možete dobiti sljedeću poruku o pogrešci: "sustava 1312 pogreška. Navedena sesija prijave ne postoji. Je možda već prekinuta"kada pokrenete **neto koristi** za račun servisa sustava/mreže. Ako se to dogodi, provjerite sadrži li korisničko ime proslijeđenim **neto** korištenje informacija o domeni li (na primjer: "[naziv računa spremišta]. file.core.windows .net").

## <a name="linux-client-problems"></a>Problemi Linux klijenta

<a id="encryption"></a>
### <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Pogreška "Kopirate datoteke na odredište koje ne podržava šifriranje"

#### <a name="cause"></a>Uzrok

BitLocker šifrirane datoteke mogu kopirati Azure datotekama. Međutim, za pohranu datoteka ne podržava NTFS EFS. Dakle, vjerojatno koristite EFS u tom slučaju. Ako imate datoteke koje su šifrirane kroz EFS, kopiranja za pohranu datoteka uspijeva, osim ako naredbu Kopiraj je dešifriranja kopirane datoteke.

#### <a name="workaround"></a>Zaobilazno rješenje

Da biste kopirali datoteke za pohranu datoteka, morate najprije dešifrirati ga. To možete učiniti pomoću neke od sljedećih načina:

• Pomoću **/d Kopiraj**.

• Postavite sljedećeg ključa registra:

- Put = HKLM\Software\Policies\Microsoft\Windows\System
- Vrijednost upišite = DWORD
- Naziv = CopyFileAllowDecryptedRemoteDestination
- Vrijednost = 1

Međutim, imajte na umu da postavka ključa registra utječe na sve operacije kopiranja s mrežom zajednički koristi.

<a id="errorhold"></a>
### <a name="host-is-down-error-on-existing-file-shares-or-the-shell-hangs-when-you-run-list-commands-on-the-mount-point"></a>Dijeli "Glavno računalo nije dostupno" Pogreška na postojeću datoteku ili ljuske blokira kada pokrenete popis naredbi na točku postavljanja

#### <a name="cause"></a>Uzrok

Kada klijent neaktivno dulje vrijeme, ta se pogreška se pojavljuje na klijentskom računalu Linux. Kada se pojavi ta pogreška, klijent prekida vezu, a ističe veze klijenta.

#### <a name="solution"></a>Rješenja

Taj se problem sada je fiksiran otklanjanje Linux kao dio [promijenite skup](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93), na čekanju backport u raspodjele Linux.

Da biste funkcioniranja putem riješili problem, nastavka rješenja za povezivanje i izbjegli pojavljivanje u stanju mirovanja, zadržavanje datoteke u odjeljku zajedničko korištenje datoteka Azure koje pišete povremeno. To mora biti postupak pisanja, kao što su upisivanjem datum stvaranja/izmjene na datoteci. U suprotnom, mogla bi vam se predmemorirani rezultate, a postupak može pokrenuti vezu.

<a id="error15"></a>
### <a name="mount-error-115-when-you-try-to-mount-azure-files-on-the-linux-vm"></a>"Postavljanje pogreške 115" prilikom pokušaja postavljanja Azure datoteka na Linux VM

#### <a name="cause"></a>Uzrok

Distribucija Linux još podržava šifriranje u SMB 3.0. U nekim distribucija korisnik dobiti poruku pogreške "115" ako pokušaju postavljanja Azure datoteke pomoću SMB 3.0 zbog značajku koja nedostaje.

#### <a name="solution"></a>Rješenja

Ako Linux SMB klijent koji se koristi ne podržava šifriranje, postavljanja Azure datoteka pomoću SMB 2.1 iz Linux VM na istom podatkovnog centra kao račun za pohranu datoteka.

<a id="delayproblem"></a>
### <a name="linux-vm-experiencing-random-delays-in-commands-like-ls"></a>Linux VM pojave slučajni kašnjenja u naredbama kao što su "ls"

#### <a name="cause"></a>Uzrok

To se može dogoditi prilikom postavljanja naredba ne obuhvaća mogućnost **serverino** . Bez **serverino**ls pokreće se naredba **stat** na sve datoteke.

#### <a name="solution"></a>Rješenja

Provjera **serverino** u unos "/ itd/fstab":

azureuser.File.Core.Windows.NET/WMS/comer na cifs sampledir vrsta/Polazno / (rw, nodev, relatime, vers = 2.1, sec = ntlmssp, predmemorije = izričite, korisničko ime = xxx, domene = X file_mode = 0755, dir_mode = 0755 serverino, rsize = 65536, wsize = 65536, actimeo = 1)

Ako mogućnost **serverino** nema, skidanje i postavljanja Azure ponovno datoteke tako da vas odabrana mogućnost **serverino** .

## <a name="learn-more"></a>uči više

- [Početak rada s Azure pohrani u sustavu Windows](storage-dotnet-how-to-use-files.md)

- [Početak rada s Azure pohrani na Linux](storage-how-to-use-files-linux.md)
