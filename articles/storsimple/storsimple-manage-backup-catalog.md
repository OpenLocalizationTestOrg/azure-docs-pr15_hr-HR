<properties 
   pageTitle="Upravljanje katalog sigurnosne kopije StorSimple | Microsoft Azure"
   description="U članku se objašnjava korištenje stranice za katalog StorSimple Upravitelj servisa popisa, odaberite i brisanje sigurnosne kopije skupovi za jedinicu."
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
   ms.date="04/28/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>Koristite StorSimple Upravitelj servisa za upravljanje katalogu sigurnosnog kopiranja

## <a name="overview"></a>Pregled

Stranica za **Katalog** StorSimple Upravitelj servisa prikazuje sve skupove sigurnosne kopije stvorene kada uzimaju se ručno ili zakazano sigurnosno kopiranje. Koristite ovu stranicu da biste popis svih sigurnosnih kopija za sigurnosne kopije pravila ili količine, odaberite ili brisanje sigurnosne kopije ili koristite sigurnosnu kopiju da biste vratili ili Kloniraj jedinicu.

Pomoću ovog praktičnog vodiča objašnjava kako popisa, odaberite i brisanje sigurnosne kopije skupa. Da biste saznali kako vratiti uređaju iz sigurnosne kopije, otvorite da biste [vratili uređaj za sigurnosno kopiranje](storsimple-restore-from-backup-set.md). Da biste saznali kako Kloniraj jedinicu, idite na [Kloniraj StorSimple jedinicu](storsimple-clone-volume.md).

![Katalog](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

Stranica **Katalog** pruža upita da biste suzili sigurnosnu kopiju postavljanje odabira. Možete filtrirati sigurnosne kopije skupova koji dohvaćaju, na temelju sljedećih parametara:

- **Uređaj** – uređaja na kojem je stvorena skup sigurnosnih kopija.

- **Sigurnosne kopije pravila ili količine** – sigurnosne kopije pravila ili glasnoću povezan s ovog sigurnosnog skupa.

- **Od i do** – raspon datuma i vremena prilikom stvaranja skupa sigurnosne kopije.

Filtrirani sigurnosne kopije skupove se zatim pozivu ovisno o sljedećim atributima:

- **Naziv** – naziv sigurnosne kopije pravila ili glasnoću pridružene skup sigurnosnih kopija.

- **Veličina** – stvarnoj veličini skupa sigurnosne kopije.

- **Stvaranja** – datum i vrijeme kada su stvoreni sigurnosnih kopija. 

- **Vrsta** – skupove sigurnosne kopije može biti lokalni snimke ili brze snimke u oblaku. Lokalni snimke je sigurnosne kopije svih podataka glasnoću spremiti lokalno na uređaj, dok se snimka oblaka se odnosi na sigurnosnu kopiju podataka glasnoću residing u oblaku. Lokalni snimke omogućuju brži pristup dok snimke oblaka su odabrali za otpornost podataka.

- **Pokrenuo** – sigurnosnih kopija može se inicirati automatski raspored ili ručno korisnika. Sigurnosne kopije pravila možete koristiti da biste zakazali sigurnosne kopije. Umjesto toga, možete koristiti mogućnost **poduzeti sigurnosne kopije** da bi ručno sigurnosno kopiranje.

## <a name="list-backup-sets-for-a-volume"></a>Popis skup sigurnosnih kopija za jedinicu
 
Provedite sljedeće korake da biste dobili popis svih sigurnosnih kopija za jedinicu.

#### <a name="to-list-backup-sets"></a>Na popisu sigurnosne kopije skupove

1. Na stranici servisa StorSimple Upravitelj karticu **kataloga sigurnosnu kopiju** .

2. Filtriranje odabira na sljedeći način:

    1. Odaberite odgovarajući uređaj.

    2. Na padajućem popisu odaberite jedinice da biste vidjeli odgovara sigurnosnih kopija.

    3. Odredite raspon vremena.

    4. Kliknite ikonu provjeri ![Ikonu Provjera](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) za izvođenje ovog upita.
 
    Sigurnosno kopiranje pridružene Odabrana jedinica prikazivati na popis skupova sigurnosne kopije.

## <a name="select-a-backup-set"></a>Odaberite skup sigurnosnih kopija

Provedite sljedeće korake da biste odabrali sigurnosnu kopiju postavljena za količinsko ili sigurnosne kopije pravila.

#### <a name="to-select-a-backup-set"></a>Da biste odabrali skup sigurnosnih kopija

1. Na stranici servisa StorSimple Upravitelj karticu **kataloga sigurnosnu kopiju** .

2. Filtriranje odabira na sljedeći način:

    1. Odaberite odgovarajući uređaj.

    2. Na padajućem popisu odaberite volumen ili sigurnosne kopije pravila za sigurnosne kopije koju želite odabrati.

    3. Odredite raspon vremena.

    4. Kliknite ikonu provjeri ![Ikonu Provjera](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) za izvođenje ovog upita.

    Sigurnosnih kopija pridružene Odabrana jedinica ili sigurnosne kopije pravila prikazivati na popisu skupa sigurnosne kopije.

3. Odaberite, a zatim proširite skup sigurnosne kopije. Pri dnu stranice prikazat će se mogućnosti **vratiti** , a zatim **Izbriši** . Poduzimanjem bilo kojeg od tih možete izvoditi na skup sigurnosne kopije koju ste odabrali.

## <a name="delete-a-backup-set"></a>Brisanje sigurnosne kopije skupa

Brisanje sigurnosne kopije kada više ne želite zadržati podatke povezane s njom. Izvršite sljedeće korake da biste izbrisali skup sigurnosne kopije.

#### <a name="to-delete-a-backup-set"></a>Da biste izbrisali skup sigurnosnih kopija

1. Na stranici upravitelja StorSimple servisa kliknite **karticu kataloga sigurnosnu kopiju**.

2. Filtriranje odabira na sljedeći način:

    1. Odaberite odgovarajući uređaj.

    2. Na padajućem popisu odaberite volumen ili sigurnosne kopije pravila za sigurnosne kopije koju želite odabrati.

    3. Odredite raspon vremena.

    4. Kliknite ikonu provjeri ![Ikonu Provjera](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) za izvođenje ovog upita.

    Sigurnosnih kopija pridružene Odabrana jedinica ili sigurnosne kopije pravila prikazivati na popisu skupa sigurnosne kopije.

3. Odaberite, a zatim proširite skup sigurnosne kopije. Pri dnu stranice prikazat će se mogućnosti **vratiti** , a zatim **Izbriši** . Kliknite **Izbriši**.

4. Bit ćete obaviješteni prilikom brisanja je u tijeku i kada je uspješno dovršena. Po završetku brisanje osvježavanje upita na ovoj stranici. Izbrisane skup sigurnosnih kopija više se ne prikazuje na popisu skupa sigurnosne kopije.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [koristiti katalog da biste vratili uređaj za sigurnosno kopiranje](storsimple-restore-from-backup-set.md).

- Saznajte kako [pomoću upravitelja StorSimple servisa za administraciju uređaju StorSimple](storsimple-manager-service-administration.md).
