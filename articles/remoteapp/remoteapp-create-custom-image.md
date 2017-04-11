<properties
    pageTitle="Kako stvoriti prilagođeni predložak slika za Azure RemoteApp | Microsoft Azure"
    description="Saznajte kako stvoriti prilagođeni predložak sliku za Azure RemoteApp. Ovaj predložak možete koristiti s hibridnog ili oblačić zbirku."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-custom-template-image-for-azure-remoteapp"></a>Kako stvoriti prilagođeni predložak slika za Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Azure RemoteApp koristi predložak sliku Windows Server 2012 R2 za hostiranje sve programe koji želite zajednički koristiti s korisnicima. Da biste stvorili prilagođenu sliku RemoteApp predložak, možete započeti s postojeće slike ili stvorite novi. 


> [AZURE.TIP] Jeste li znali sliku možete stvoriti iz programa VM Azure? TRUE priče i izrezuje prema dolje na vrijeme je traje da biste uvezli sliku. Pogledajte korake [u nastavku](remoteapp-image-on-azurevm.md).

Preduvjeti za sliku koja je moguće prenijeti za Azure RemoteApp su:


- Veličina slike mora biti višekratnik broja MB prostora. Ako pokušaju prenijeti sliku koja nije višekratnik prijenos neće uspjeti.
- Veličina slike mora biti 127 GB ili manje.
- Mora biti u datoteci VHD (VHDX datoteke [Hyper-V virtualne napravili pogona] trenutno nisu podržani).
- Na VHD mora biti generacije 2 virtualnog računala.
- Na VHD može biti-veličine ili dinamički širi. Dinamički širi VHD preporučuje se jer je potrebno manje vremena da biste prenijeli na Azure od-veličina datoteke VHD.
- Na disku morate pokrenuti pomoću matrice pokretanje zapis (MBR) particija stil. Stil particije tablice (GPT) particije GUID nije podržana.
- Na VHD mora sadržavati jednu instalaciju sustava Windows Server 2012 R2. Može sadržavati više jedinica, ali samo jedan koji sadrži instalacija sustava Windows.
- Mora biti instaliran ulogu udaljene radne površine sesiju glavnog računala (RDSH) i značajku doživljaja radne površine.
- Morate ulogu Broker veze udaljene radne površine *nije* moguće instalirati.
- Šifriranje datotečni sustav (EFS) mora biti onemogućena.
- Slika mora biti SYSPREPed pomoću parametre **/OOBE / generalize Shutdown** (koji ne koristi parametar **/mode:vm** ).
- Prijenos na VHD iz snimke lanac nisu podržani.


**Prije početka**

Trebali biste učiniti sljedeće prije stvaranja usluge:

- [Registracija](https://azure.microsoft.com/services/remoteapp/) za RemoteApp.
- Stvorite korisnički račun u servisu Active Directory da biste koristili kao račun servisa RemoteApp. Ograničite dozvole za taj račun tako da se mogu samo pridružiti strojeva domeni. Dodatne informacije potražite u članku [Konfiguriranje Azure Active Directory za RemoteApp](remoteapp-ad.md) .
- Prikupite informacije o lokalne mreže: IP adresa informacije i Detalji o uređaju VPN-a.
- Instalirajte modul [Azure PowerShell](../powershell-install-configure.md) .
- Prikupite informacije o korisnicima koji želite dopustiti pristup. To se može biti podatke o Microsoftovu računu ili servisa Active Directory podatke o računu tvrtke za korisnike.



## <a name="create-a-template-image"></a>Stvaranje predloška slike ##

Ovo su više razine korake da biste stvorili novu sliku predložak iznova:

1.  Pronađite sliku Windows Server 2012 R2 ažuriranje DVD-a ili ISO.
2.  Stvaranje datoteke VHD.
4.  Instalirajte Windows Server 2012 R2.
5.  Instalirajte ulogu udaljene radne površine sesiju glavnog računala (RDSH) i značajku doživljaja radne površine.
6.  Instaliranje dodatnih značajki potrebnih aplikacija.
7.  Instalirajte i konfigurirajte aplikacije. Da biste olakšali zajedničkog korištenja aplikacije, dodajte sve aplikacije ili više njih koje želite omogućiti zajedničko korištenje na izbornik **Start** slike, posebice u **%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs.
8.  Izvođenje bilo koje dodatna konfiguracija Windows potrebnih aplikacija.
9.  Onemogućivanje šifriranja datotečni sustav (EFS).
10. **Potrebna:** Otvorite Windows Update i instalirajte sva važna ažuriranja.
9.  SYSPREP slike.

Detaljne upute za stvaranje nove slike su:

1.  Pronađite i Windows Server 2012 R2 ažuriranje DVD-a ili ISO sliku.
2.  Stvaranje datoteke VHD pomoću upravljanja Disk.
    1.  Pokrenite Disk Management (diskmgmt.msc).
    2.  Stvaranje dinamički širi VHD 40 GB ili više veličine. (Procjenu razmak potreban za Windows, aplikacije i prilagodbe. Windows Server s RDSH uloge i značajku doživljaja radne površine instaliran je potrebno oko 10 GB prostora).
        1.  Kliknite **Akcije > Stvori VHD**.
        2.  Navedite mjesto, veličinu i oblik VHD. Odaberite **dinamično proširivanje**, a zatim kliknite **u redu**.

            To će pokrenuti nekoliko sekundi. Nakon dovršetka stvaranja VHD, trebali biste vidjeti novi disk bez sve slovo i u stanju "Nije pokrenut" na konzoli za upravljanje Disk.

        - Desnom tipkom miša kliknite disk (ne Nedodijeljeni razmak), a zatim kliknite **Pokretanje diska**. Odaberite **MBR** (glavni zapis) kao Stil particije, a zatim kliknite **u redu**.
        - Stvaranje nove jedinice: desnom tipkom miša kliknite nedodijeljeni prostor, a zatim kliknite **Nova jednostavna jedinica**. Prihvatite zadane postavke u čarobnjaku ali pripazite dodijeliti slovo da biste izbjegli probleme prilikom učitavanja slike predložak.
        - Desnom tipkom miša kliknite disk, a zatim kliknite **Odvajanje VHD**.





1. Instalirajte Windows Server 2012 R2:
    1. Stvaranje novog virtualnog računala. Pomoću čarobnjaka za novo virtualnog računala u Hyper-V Upravitelj ili klijenta Hyper-V.
        1. Na stranici Određivanje generacije odaberite **generacije 1**.
        2. Na stranici povezivanje virtualne na tvrdom disku, odaberite **koristi postojeće virtualne tvrdog diska**, a pronađite VHD koji ste stvorili u prethodnom koraku.
        2. Na stranici mogućnosti instalacije odaberite **instalirati operacijski sustav iz pokretanje CD-DVD_ROM**, a zatim mjesto medij za instalaciju sustava Windows Server 2012 R2.
        3. U čarobnjaku koji su potrebne za instalaciju sustava Windows i aplikacije, odaberite druge mogućnosti. Dovršite čarobnjak.
    2.  Nakon dovršetka postupka čarobnjaka uređivanje postavki virtualnog računala i sve druge promjene potrebne za instalaciju sustava Windows i programa, kao što su broj procesora koji se virtualne, a zatim **u redu**.
    4.  Povezivanje s virtualnog računala i instalirajte Windows Server 2012 R2.
1. Instalirajte ulogu udaljene radne površine sesiju glavnog računala (RDSH) i značajku doživljaja radne površine:
    1. Pokrenite Upravitelj poslužitelja.
    2. Kliknite **Dodavanje uloge i značajke** na zaslonu dobrodošlice ili na izborniku **Upravljanje** .
    3. Kliknite **Dalje** na stranici prije nego što počnete.
    4. Odaberite **ulogu ili značajku instalaciju**, a zatim kliknite **Dalje**.
    5. Na popisu odaberite na lokalnom računalu, a zatim kliknite **Dalje**.
    6. Odaberite **Udaljene radne površine**, a zatim kliknite **Dalje**.
    7. Proširite **korisnička sučelja i infrastrukturu** i odaberite **Doživljaj radne površine**.
    8. Kliknite **Dodaj značajke**, a zatim kliknite **Dalje**.
    9. Na stranici udaljene radne površine, kliknite **Dalje**.
    10. Kliknite **sesiji udaljene radne površine glavnog računala**.
    11. Kliknite **Dodaj značajke**, a zatim kliknite **Dalje**.
    12. Na stranici Potvrda instalacije odabira, odaberite **automatski ako je potrebno ponovno pokrenuti odredišnom poslužitelju**, a zatim **da** na stranici upozorenje ponovnog pokretanja.
    13. Kliknite **Instaliraj**. Računalo će se pokrenuti.
1.  Instaliranje dodatnih značajki potrebnih aplikacija, kao što su .NET Framework 3.5. Da biste instalirali značajke, pokrenite čarobnjak značajke i dodavanje uloge.
7.  Instaliranje i konfiguriranje programa i aplikacijama koje želite objaviti kroz RemoteApp.

>[AZURE.IMPORTANT]
>
>Instalirajte ulogu RDSH prije instalacije aplikacije da biste bili sigurni da su probleme s kompatibilnošću aplikacije otkrio prije prijenosa slika na RemoteApp.
>
>Provjerite je li prečac za aplikaciju (**.lnk** datoteka) pojavljuje se na izborniku **Start** za sve korisnike (%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs). I provjerite je li ikonu vidite na izborniku **Start** ono što želite da korisnici vide. Ako nije, promijeniti. (Ne *ste* da biste dodali aplikaciju na početak izbornik, ali olakšava mnogo objavljivanje aplikacija RemoteApp. U suprotnom potrebno unijeti put instalacije aplikacije Kad objavite aplikaciju.)


8.  Izvođenje bilo koje dodatna konfiguracija Windows potrebnih aplikacija.
9.  Onemogućivanje šifriranja datotečni sustav (EFS). U prozoru za razmjenu povećane naredbe, pokrenite sljedeću naredbu:

        Fsutil behavior set disableencryption 1

    Osim toga, možete postaviti ili dodajte sljedeće vrijednosti DWORD u registru:

        HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
9.  Ako izrađujete vaše slike unutar Azure virtualnog računala, preimenovati u ** \%windir%\Panther\Unattend.xml** datoteku, kao što je to će onemogućiti prijenos skripta koja se koristi kasnije funkcioniranje. Promijenite naziv datoteke da biste Unattend.old tako da će i dalje imate datoteku za slučaj da morate vratiti implementaciju sustava.
10. Otvorite Windows Update i instalirajte sva važna ažuriranja. Možda ćete morati pokretanje servisa Windows Update više puta da biste preuzeli sva ažuriranja. (Ponekad instalirate ažuriranje i se ažuriranje sam je potrebno ažuriranje.)
10. SYSPREP slike. Povećani naredbenom retku pokrenite sljedeću naredbu:

    **C:\Windows\System32\sysprep\sysprep.exe / generalize/OOBE Shutdown**

    **Bilješke:** Upotrijebite parametar **/mode:vm** naredbe SYSPREP čak i ako je to virtualnog računala.


## <a name="next-steps"></a>Daljnji koraci ##
Sad kad imate sliku prilagođeni predložak, morate prenijeti tu sliku u zbirku RemoteApp. Da biste stvorili zbirci web, poslužite se informacijama u sljedećim člancima:


- [Kako stvoriti hibridnog skup RemoteApp](remoteapp-create-hybrid-deployment.md)
- [Kako stvoriti oblaka skup RemoteApp](remoteapp-create-cloud-deployment.md)
 