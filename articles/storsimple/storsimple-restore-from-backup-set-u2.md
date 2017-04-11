<properties 
   pageTitle="Vraćanje StorSimple jedinice iz sigurnosne kopije | Microsoft Azure"
   description="U članku se objašnjava korištenje stranice za katalog StorSimple Upravitelj servisa da biste vratili StorSimple jedinice iz sigurnosne kopije skupa."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/26/2016"
   ms.author="v-sharos" />

# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Vraćanje StorSimple jedinice iz sigurnosne kopije skupa (ažuriranje 2)

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Pregled

Stranica za **Katalog** prikazuje sve skupove sigurnosne kopije stvorene kada uzimaju se ručno i automatsko sigurnosno kopiranje. Koristite ovu stranicu da biste popis svih sigurnosnih kopija za sigurnosne kopije pravila ili količine, odaberite ili brisanje sigurnosne kopije ili koristite sigurnosnu kopiju da biste vratili ili Kloniraj jedinicu.

 ![Sigurnosno kopiranje stranica kataloga](./media/storsimple-restore-from-backup-set-u2/restore.png)

Pomoću ovog praktičnog vodiča objašnjava kako pomoću **Katalog** stranice da biste vratili uređaj za sigurnosno kopiranje.

Možete vratiti volumen iz lokalne ili sigurnosne kopije u oblak. U svakom slučaju, postupak vraćanja poziva glasnoće Internetu odmah dok podataka se preuzima u pozadini. 

Prije nego što pokrenete postupak vraćanja, imajte na umu sljedeće:

- **Morate poduzeti glasnoće izvanmrežno** – preuzimanje glasnoće izvanmrežno na glavnom računalu i uređaj prije nego što započeli postupak vraćanja. Iako se postupak vraćanja automatski unosi glasnoće online na uređaju, morate ručno premjestiti uređaj na Internetu na glavnom računalu. Glasnoću online možete prenijeti na računalo koje hostira čim Glasnoća je online na uređaju. (Ne morate čekati da se završi postupak vraćanja.) Postupak, otvorite da biste [preuzeli glasnoću izvanmrežno](storsimple-manage-volumes-u2.md#take-a-volume-offline).

- **Jedinica vrsta nakon vraćanja** – izbrisani količine se vraćaju ovise o vrsti u snimku; to jest, količine koje lokalno prikvačene se vraćaju kao lokalno prikvačene količine i količine koje su tiered se vraćaju kao tiered jedinice.

    Za postojeće jedinicama Trenutna vrsta korištenje jedinice nadjačava vrstu koji je spremljen u snimke. Ako, na primjer, ako vraćate jedinice iz snimku zaslona koja je izvedena kada je tiered vrstu glasnoću i jedinica vrsta sada lokalno prikvačene (zbog pretvaranja postupak koji izvršena), zatim glasnoću vratit će se kao lokalno prikvačene jedinica. Isto tako, ako postojeću lokalno prikvačene jedinicu je proširen i naknadno vratiti iz starije brze snimke izvedena kada je glasnoću manje, vraćena glasnoću će zadržati trenutna prošireno veličina.

    Ne mogu pretvoriti jedinice iz tiered glasnoću lokalno prikvačene jedinicu ili lokalno prikvačene glasnoću tiered jedinicu dok vraća glasnoću. Pričekajte da se završi postupak vraćanja, a zatim glasnoću možete pretvoriti u neku drugu vrstu. Informacije o pretvaranju jedinicu, prijeđite na odjeljak [Promjena vrste glasnoću](storsimple-manage-volumes-u2.md#change-the-volume-type). 

- **Veličina jedinice odrazit će se u vraćenu glasnoću** – to je važna činjenica Ako vraćate lokalno prikvačene glasnoću koji je izbrisan (jer lokalno prikvačene količine potpuno su tamo). Provjerite imate li dovoljno prostora prije nego se pokušate da biste vratili lokalno prikvačene jedinicu koju prethodno izbrisali. 

- **Nije moguće proširiti glasnoću dok se vraćaju** – Pričekajte postupak vraćanja dovršetka prije nego se pokušate da biste proširili jedinicu. Informacije o Proširivanje jedinice, idite na [Izmijeni jedinicu](storsimple-manage-volumes-u2.md#modify-a-volume).

- **Na raspolaganju vam sigurnosnu kopiju dok se vraćate na lokalnu jedinicu** – postupak idite na članak [Korištenje StorSimple Upravitelj servisa za upravljanje sigurnosne kopije pravila](storsimple-manage-backup-policies.md).

- **Možete otkazati postupak vraćanja** – Ako odustanete zadatak obnavljanja, a zatim glasnoću bit će vraćen stanje u kojem je bila prije nego što ste pokrenuli postupak vraćanja. Postupak, otvorite da biste [poništili posao](storsimple-manage-jobs-u2.md#cancel-a-job).

## <a name="how-to-use-the-backup-catalog"></a>Kako koristiti katalog sigurnosne kopije

Stranica **Katalog** pruža upita koji olakšava da biste suzili sigurnosnu kopiju postavljanje odabira. Možete filtrirati sigurnosne kopije skupovi koji dohvaćaju temelju sljedećih parametara:

- **Uređaj** – uređaja na kojem je stvorena skup sigurnosnih kopija.
- **Sigurnosne kopije pravila** ili **glasnoću** – sigurnosne kopije pravila ili glasnoću povezan s ovog sigurnosnog skupa.
- **Iz** te **Da biste** – raspon datuma i vremena prilikom stvaranja skupa sigurnosne kopije.

Filtrirani sigurnosne kopije skupove se zatim pozivu ovisno o sljedećim atributima:

- **Naziv** – naziv sigurnosne kopije pravila ili glasnoću pridružene skup sigurnosnih kopija.
- **Veličina** – stvarnoj veličini skupa sigurnosne kopije.
- **Stvaranja** – datum i vrijeme kada su stvoreni sigurnosnih kopija. 
- **Vrsta** – skupove sigurnosne kopije može biti lokalni snimke ili brze snimke u oblaku. Lokalni snimke je sigurnosne kopije svih podataka glasnoću spremiti lokalno na uređaj, dok se snimka oblaka se odnosi na sigurnosnu kopiju podataka glasnoću residing u oblaku. Lokalni snimke omogućuju brži pristup dok snimke oblaka su odabrali za otpornost podataka.
- **Pokrenuo** – sigurnosnih kopija može se inicirati automatski prema rasporedu ili ručno korisnik. (Možete koristiti sigurnosne kopije pravila da biste zakazali sigurnosne kopije. Osim toga, možete koristiti mogućnost **poduzeti sigurnosnu kopiju** da biste preuzeli interaktivnu sigurnosnu kopiju.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>Kako vratiti glasnoću StorSimple iz sigurnosne kopije

**Katalog** stranice možete koristiti da biste vratili glasnoću StorSimple iz određene sigurnosnu kopiju. Imajte na umu, taj vraćanje jedinice će glasnoće u stanje u kojem je bio kad je izvršena sigurnosnu kopiju. Izgubit će se sve podatke koji je dodan nakon postupka sigurnosne kopije.

> [AZURE.WARNING] Vraćanje iz sigurnosne kopije zamijenit će postojeće količine iz sigurnosne kopije. To može uzrokovati gubitak podatke napisane nakon snimljena sigurnosnu kopiju.

### <a name="to-restore-your-volume"></a>Da biste vratili glasnoću

1. Na stranici servisa StorSimple Upravitelj karticu **kataloga sigurnosnu kopiju** .

    ![Katalog](./media/storsimple-restore-from-backup-set-u2/restore.png)

2. Odaberite sigurnosne kopije postavite na sljedeći način:
  1. Odaberite odgovarajući uređaj.
  2. Na padajućem popisu odaberite volumen ili sigurnosne kopije pravila za sigurnosne kopije koju želite odabrati.
  3. Odredite raspon vremena.
  4. Kliknite ikonu provjeri ![Provjera ikona](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) za izvođenje ovog upita.
 
    Sigurnosnih kopija pridružene Odabrana jedinica ili sigurnosne kopije pravila prikazivati na popisu skupa sigurnosne kopije.

3. Proširite skup da biste vidjeli povezane količine sigurnosnih kopija. Ove jedinice mora biti izvanmrežno na glavnom računalu, a uređaj prije no što ih možete vratiti. Pristup količine na stranici **Spremnika glasnoće** , a zatim slijedite korake u [poduzeti glasnoću izvan mreže](storsimple-manage-volumes-u2.md#take-a-volume-offline) da biste ih izvanmrežni.

    > [AZURE.IMPORTANT] Provjerite da su najprije obavljali količine izvanmrežno na glavnom računalu, prije nego količine izvanmrežno na uređaju. Ako se neće moći koristiti izvanmrežno količine na glavnom računalu, potencijalno može dovesti oštećenja podataka.

4. Vratite se na kartici **Katalog** , a zatim odaberite skup sigurnosne kopije.

5. Kliknite **Vrati** pri dnu stranice.

6. Zatražit će se za potvrdu. Pregledajte podatke vraćanje, a zatim odaberite potvrdni okvir za potvrdu.

    ![Stranici za potvrdu](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)

7. Kliknite ikonu provjeri ![provjerite ikona](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). To će pokrenuti vraćanje zadatak koji možete vidjeti tako da pristupa stranici **Poslovi** . 

8. Po dovršetku vraćanja možete provjeriti sadržaj diskova se zamijene količine iz sigurnosne kopije.

![Videozapis dostupne](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **videozapis dostupna**

Da biste pogledali videozapis u kojem se pokazuje kako možete koristiti u Kloniraj i vraćanje značajke u StorSimple da biste oporavili izbrisane datoteke, kliknite [ovdje](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Ako ne uspije vraćanja

Primit ćete upozorenje ne uspijete postupak vraćanja iz bilo kojeg razloga. U tom slučaju osvježavanje sigurnosne kopije popisa da biste provjerili je li još uvijek valjan sigurnosnu kopiju. Ako vrijedi sigurnosnog kopiranja i vraćate iz oblaka, zatim problema s povezivanjem uzrokuju problem. 

Da biste dovršili postupak vraćanja, iskoristite glasnoće izvanmrežno na glavnom računalu i ponovite postupak vraćanja. Imajte na umu da sve izmjene glasnoću podataka koji su izvesti tijekom postupka Vrati se izgubiti.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [upravljati StorSimple količine](storsimple-manage-volumes-u2.md).

- Saznajte kako [pomoću upravitelja StorSimple servisa za administraciju uređaju StorSimple](storsimple-manager-service-administration.md).
