<properties 
   pageTitle="Vraćanje StorSimple jedinice iz sigurnosne kopije | Microsoft Azure"
   description="U članku se objašnjava korištenje stranice za katalog StorSimple Upravitelj servisa da biste vratili StorSimple jedinice iz sigurnosne kopije skupa."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Vraćanje StorSimple jedinice iz skupa sigurnosne kopije

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Pregled

Stranica za **Katalog** prikazuje sve skupove sigurnosne kopije stvorene kada uzimaju se ručno i automatsko sigurnosno kopiranje. Koristite ovu stranicu da biste popis svih sigurnosnih kopija za sigurnosne kopije pravila ili količine, odaberite ili brisanje sigurnosne kopije ili koristite sigurnosnu kopiju da biste vratili ili Kloniraj jedinicu.

 ![Sigurnosno kopiranje stranica kataloga](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

Pomoću ovog praktičnog vodiča objašnjava kako pomoću **Katalog** stranice da biste vratili glasnoću na uređaju iz sigurnosne kopije skupa.

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

**Katalog** stranice možete koristiti da biste vratili glasnoću StorSimple iz određene sigurnosnu kopiju. 

> [AZURE.WARNING] Vraćanje iz sigurnosne kopije zamijenit će postojeće količine iz sigurnosne kopije. To može uzrokovati gubitak podatke napisane nakon snimljena sigurnosnu kopiju.

Prije nego što pokrenete vraćanja na jedinicu, provjerite je li glasnoća je izvan mreže. Morat ćete poduzeti izvanmrežno glasnoće na glavnom računalu prvi, a zatim željeni uređaj. Slijedite korake u [poduzeti glasnoću izvanmrežno](storsimple-manage-volumes.md#take-a-volume-offline). Izvršite sljedeće korake da biste vratili jedinice iz sigurnosne kopije skupa.

### <a name="to-restore-from-a-backup-set"></a>Vraćanje iz sigurnosne kopije skupa

1. Na stranici servisa StorSimple Upravitelj karticu **kataloga sigurnosnu kopiju** .

    ![Katalog](./media/storsimple-restore-from-backup-set/HCS_Restore.png)

2. Odaberite sigurnosne kopije postavite na sljedeći način:
  1. Odaberite odgovarajući uređaj.
  2. Na padajućem popisu odaberite volumen ili sigurnosne kopije pravila za sigurnosne kopije koju želite odabrati.
  3. Odredite raspon vremena.
  4. Kliknite ikonu provjeri ![Provjera ikona](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) za izvođenje ovog upita.
 
    Sigurnosnih kopija pridružene Odabrana jedinica ili sigurnosne kopije pravila prikazivati na popisu skupa sigurnosne kopije.

3. Proširite skup da biste vidjeli povezane količine sigurnosnih kopija. Ove jedinice mora biti izvanmrežno na glavnom računalu, a uređaj prije no što ih možete vratiti. Slijedite korake u [poduzeti glasnoću izvanmrežno](storsimple-manage-volumes.md#take-a-volume-offline).

    >  [AZURE.IMPORTANT] Provjerite da su najprije obavljali količine izvanmrežno na glavnom računalu, prije nego količine izvanmrežno na uređaju. Ako se neće moći koristiti izvanmrežno količine na glavnom računalu, potencijalno može dovesti oštećenja podataka.

4. Odaberite skup sigurnosnih kopija. Kliknite **Vrati** pri dnu stranice.

6. Zatražit će se za potvrdu. 

    ![Stranici za potvrdu](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)

7. Pregledajte podatke vraćanja i kliknite ikonu provjeri ![provjerite ikona](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). To će pokrenuti vraćanje zadatak koji možete vidjeti tako da pristupa stranici **Poslovi** . 

8. Po dovršetku vraćanja možete provjeriti sadržaj diskova se zamijene količine iz sigurnosne kopije.

![Videozapis dostupne](./media/storsimple-restore-from-backup-set/Video_icon.png) **videozapis dostupna**

Da biste pogledali videozapis u kojem se pokazuje kako možete koristiti u Kloniraj i vraćanje značajke u StorSimple da biste oporavili izbrisane datoteke, kliknite [ovdje](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [upravljati StorSimple količine](storsimple-manage-volumes.md).

- Saznajte kako [pomoću upravitelja StorSimple servisa za administraciju uređaju StorSimple](storsimple-manager-service-administration.md).
