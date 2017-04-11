
<properties 
    pageTitle="Kako se Azure RemoteApp ne sprema korisničkih podataka i postavki? | Microsoft Azure"
    description="Saznajte kako Azure RemoteApp sprema korisničkih podataka pomoću profila disku korisnika."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Kako se Azure RemoteApp ne sprema korisničkih podataka i postavki?

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Azure RemoteApp sprema identiteti korisnika i prilagodbe svim uređajima i sesije. U ovom korisnički podaci se pohranjuju u po zbirci disk po korisniku, naziva na disku korisničkog profila (UPD). Na disku slijedi korisnika i osigurava korisnik ima dosljednost, bez obzira na to gdje se prijaviti. sprema 

Korisnički profil diskova su potpuno proziran korisnika – korisnicima spremanje dokumenata na svoje mape Dokumenti (na što se čini da je lokalni disk) i promijeniti njihove postavke aplikacije kao uobičajeni. U isto vrijeme sve osobne postavke zadržava prilikom povezivanja s Azure RemoteApp s bilo kojeg uređaja. Ne vidi se korisnik je svoje podatke na istom mjestu.

Svaki UPD ima 50GB prostora za pohranu na stalni i sadrži oba korisničkih podataka i aplikacije postavki. 

U nastavku specifičnosti na podataka profila korisnika.

>[AZURE.NOTE] Onemogućivanje na UPD nije potrebno? To možete učiniti tu – pogledajte članak Pavithra na blogu, [Onemogućivanje korisničkog profila diskova (UPDs) Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/)detalje.


## <a name="how-can-an-admin-get-to-the-data"></a>Kako administrator može doći do podatke?

Ako vam je potrebna za pristup podacima zbog nekog od korisnika (za oporavak Izrada ili ako korisnik napusti tvrtku), obratite se podršci za Azure i omogućavaju informacija o pretplati u zbirku i identitet korisnika. Tim za Azure RemoteApp će vam URL-a na VHD. Preuzmite tu VHD i dohvatiti sve dokumente i datoteke koje su vam je potrebna. Imajte na umu da se VHD 50GB tako da se na bit da biste je preuzeli.


## <a name="is-the-data-backed-up"></a>Se podaci sigurnosno?

Da, ne možemo spremanje sigurnosne kopije korisničkih podataka po geografsku lokaciju. Podaci se samo za čitanje i može se pristupiti na isti način kao obični podataka bio (kontakt Azure RemoteApp da biste podesili), ako je primarni podatkovnog centra u prema dolje. Podaci se kopira u stvarnom vremenu u mjesto za sigurnosne kopije i smo čuva kopije različitih verzija. Tako, na oštećenja podataka, ne možemo nećete moći vratiti prethodno poznati dobar verziju, ali ako je primarni podatkovnog centra u prema dolje, moći da biste dobili korisničke podatke s drugih mjesta.

## <a name="how-do-users-see-the-upd-on-the-server-side"></a>Kako učiniti korisnici vide u UPD na strani poslužitelja?

Svaki korisnik će automatski dobiti vlastitih direktorij na poslužitelju na kojem se mapira njihove UPD: c:\Users\username.

## <a name="whats-the-best-way-to-use-outlook-and-upd"></a>Što je najbolji način korištenja programa Outlook i UPD?

Azure RemoteApp sprema stanja programa Outlook (poštanski sandučići pst-ova) između sesije. Da biste to omogućili, moramo pst datoteke za pohranu u podataka profila korisnika (c:\users\<korisničko ime >). To je zadano mjesto za podatke, stoga ukoliko neće promijeniti mjesto, podaci će se zadržavaju između sesije.

Preporučujemo i da koristite "predmemorirani" način u programu Outlook, a koristite "poslužitelja/mrežni" način rada za pretraživanje.

Pogledajte odjeljak [ovog članka](remoteapp-outlook.md) dodatne informacije o korištenju programa Outlook i Azure RemoteApp.

## <a name="what-about-redirection"></a>Je li moguće koristiti preusmjeravanje?
Možete konfigurirati Azure RemoteApp da bi korisnici pristupati lokalnih uređaja tako da postavite [preusmjeravanje](remoteapp-redirection.md). Lokalni uređaji pa moći pristupiti podacima na na UPD.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Mogu li koristiti Moje UPD kao zajedničke mrežne mape?
ne. UPDs nije moguće koristiti kao zajedničke mrežne mape. Na UPD korisniku dostupna je samo kada je korisnik aktivno povezan Azure RemoteApp.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Ako je brisanje korisnika iz zbirke, njihove UPD izbriše?

Ne, brisanjem korisnika, ne možemo briše automatski na UPD – smo umjesto toga pohrane podataka dok je izbrisati zbirku. 90 dana nakon što izbrišete zbirci, ne možemo izbrisati sve UPDs. 

Ako trebate izbrisati s UPD iz zbirke, obratite se Azure RemoteApp – ne možemo možete izbrisati UPD naš strani.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Mogu li pristupiti korisnicima UPDs (trenutnu ili izbrisani korisnici)?

Da, ako kontakt [Azure RemoteApp](mailto:remoteappforum@microsoft.com)smo možete postaviti ste URL-om za pristup podacima. 10 sati ćete morati preuzeti svi podaci ili datoteke s na UPD prije isteka programa access.

## <a name="are-upds-available-offline"></a>Su UPDs dostupne izvanmrežno?

Desnom tipkom miša sada ne dajemo izvanmrežni pristup da biste UPDs, osim 10 sat prozor programa access prethodno opisan. To znači da koje ćemo trenutno nemaju način omogućuju vam pristup za dugu dovoljno da biste dovršili složenije zadatke, kao što su sustavom antivirusni softver na UPDs ili pristup podacima za reviziju.

## <a name="do-registry-key-settings-persist"></a>Ne zadržava postavke ključa registra
Da, ništa zapisivanje HKEY_Current_User je dio na UPD.

## <a name="can-i-disable-upds-for-a-collection"></a>Možete onemogućiti UPDs za zbirku?

Da, možete zatražiti Azure RemoteApp da biste onemogućili UPDs za pretplatu, ali ne možete tu sami. To znači da UPDs će biti onemogućena za sve zbirke u pretplate.

Možda želite onemogućiti UPDs u nekom od sljedećih situacija: 

- Morate dovršiti pristup i kontrola korisničkih podataka (za nadzor i pregled svrhe, kao što su financijske institucije).
- Imate 3 proizvođača korisničkog profila upravljanja rješenja lokalnog i želite da biste nastavili koristiti ih u Azure RemoteApp implementaciji sustava domene pridruženo. To je potrebna agent profila učitati u Zlatne sliku. 
- Ne morate sve lokalnih podataka za pohranu ili prikažete sve podatke u odjeljku zajedničko korištenje oblaka ili datoteke, a želite li kontrola spremanje podataka lokalno pomoću Azure RemoteApp.

Dodatne informacije potražite u članku [Onemogućivanje korisničkog profila diskova (UPDs) Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) .

## <a name="can-i-restrict-users-from-saving-data-to-the-system-drive"></a>Mogu li korisnicima zabranili spremanja podataka sistemski pogon?

Da, no morat ćete postaviti koji predložak slici prije stvaranja zbirke. Da biste blokirali pristup sistemski pogon, poduzmite sljedeće korake:

1. Pokrenite **gpedit.msc** na slici predložak.
2. Dođite do **Konfiguracija korisnika > Administrativni predlošci > komponente Windows > Explorer**.
3. Odaberite sljedeće mogućnosti:
    - **Skrivanje te navedeni pogona u mapi Moje računalo**
    - **Onemogućuju pristup pogona s mog računala**

## <a name="can-i-seed-upds-i-want-to-put-some-data-in-the-upd-thats-available-the-first-time-the-user-signs-in"></a>Može li se seed UPDs? Želim da biste postavili neki podaci UPD koja je dostupna korisnikove prijave u prvi put.

Da, prilikom stvaranja predloška sliku možete dodati informacije na zadani profil. Te podatke pa se dodaje na UPD.

## <a name="can-i-change-the-size-of-the-upd-depending-on-how-much-data-i-want-to-store"></a>Mogu li promijeniti veličinu UPD ovisno o tome koliko je podataka koje želim spremiti?

Ne, sve UPDs imate 50 GB prostora za pohranu. Ako želite spremiti različite količine podataka, pokušajte sljedeće:

1. Onemogućivanje UPDs zbirke.
2. Postavljanje zajedničke datoteke za korisnike koji žele pristupati.
3. Učitavanje zajedničko korištenje datoteka pomoću skriptu za pokretanje. Pojedinosti o skripte za pokretanje Azure RemoteApp potražite u nastavku.
4. Izravni korisnika da biste spremili sve podatke za zajedničko korištenje datoteka.


## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Kako pokrenuti skriptu za pokretanje RemoteApp Azure?

Ako želite pokrenuti skriptu za pokretanje, započnite stvaranjem zakazani zadatak na slici predložak namjeravate koristiti za zbirku. (Učinite ovo *prije nego što* pokrenete sysprep.) 

![Stvaranje zadatka sustava](./media/remoteapp-upd/upd1.png)

![Stvaranje zadatka sustava koja se pokreće kada korisnik prijavi](./media/remoteapp-upd/upd2.png)

Na kartici **Općenito** provjerite da biste promijenili **Korisnički račun** u odjeljku Sigurnost da biste "BUILTIN\Users."

![Promjena za korisnički račun u grupu](./media/remoteapp-upd/upd4.png)

Zakazani zadatak će se pokrenuti skriptu pokretanje pomoću vjerodajnica korisnika. Zakazivanje zadataka da biste pokrenuli svaki put kada korisnik prijavi.

![Postavljanje okidača za zadatak kao "pri prijavi"](./media/remoteapp-upd/upd3.png)

Možete koristiti i [skripte za pokretanje utemeljen na pravila grupe](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx). 

## <a name="what-about-placing-a-startup-script-in-the-start-menu-would-that-work"></a>Što je s smještanje skriptu za pokretanje na izborniku Start? Činite to funkcionira?

Drugim riječima, možete li stvaranje .bat datoteke koja se pokreće skriptu prozor config i spremite ga u mapu Menu\Programs\StartUp c:\ProgramData\Microsoft\Windows\Start, a zatim odredite te skripte pokrenuti svaki put kada korisnik započne sesiju RemoteApp?

Ne, koji ne podržava Azure RemoteApp koji koristi RDSH koji ne podržavaju skripte za pokretanje na izborniku Start.

## <a name="can-i-use-mstscexe-the-remote-desktop-program-to-configure-logon-scripts"></a>Mogu li koristiti mstsc.exe (program udaljene radne površine) da biste konfigurirali skripti prijave?

Nope, ne podržava Azure RemoteApp.

## <a name="can-i-store-data-on-the-vm-locally"></a>Mogu li spremati podatke na VM lokalno?

NE, podatke pohranjene bilo gdje na VM osim u na UPD izgubit će se. Postoji visoke izgledi korisnik će dobiti isti VM sljedeći put prijavite Azure RemoteApp. Da bi korisnik će prijavite se u istu VM i izgubit će se podaci ne možemo održava postojanost korisnika VM. Osim toga, prilikom ažuriranja zbirke, postojeće VMs zamjenjuje novi skup VMs – znači da se gube sve podatke pohranjene na VM sam. Na preporuka je da biste pohranili podatke u UPD, zajedničkog prostora za pohranu kao što Azure datoteke, datotečnom poslužitelju unutar na VNET ili u oblak pomoću sustava za pohranu oblaka kao što su zajedničke mrežne mape.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Način postavljanja Azure zajedničkom na VM, pomoću komponente PowerShell?

Koristite cmdlet neto PSDrive postavljanja pogonu, na sljedeći način:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


Vjerodajnice možete spremiti i tako da pokrenete sljedeće:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Omogućuju preskakanje parametar - vjerodajnica u cmdleta New-PSDrive.
