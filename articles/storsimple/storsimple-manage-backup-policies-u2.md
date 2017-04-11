<properties 
   pageTitle="Upravljanje pravilima sigurnosnu kopiju vašeg StorSimple | Microsoft Azure"
   description="U članku se objašnjava kako koristiti StorSimple Upravitelj servisa za stvaranje i upravljanje ručno sigurnosno kopiranje, sigurnosno kopiranje rasporede i sigurnosne kopije zadržavanju."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/10/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-manage-backup-policies-update-2"></a>Koristite StorSimple Upravitelj servisa za upravljanje sigurnosne kopije pravila (ažuriranje 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča objašnjava kako pomoću stranici StorSimple Upravitelj servisa **Sigurnosne kopije pravila** da biste odredili postupaka sigurnosnog kopiranja i sigurnosne kopije zadržavanja za StorSimple diskova. Opisuje se i način da biste dovršili ručno sigurnosno kopiranje.

Kada stvarate sigurnosne kopije jedinice, možete odabrati da biste stvorili lokalnu snimke ili snimku oblaka. Ako se sigurnosno lokalno prikvačene glasnoće, preporučujemo da navedete snimku oblaka. Pisanje velikog broja lokalne snimke lokalno prikvačene jedinice povezano sa skupom podataka koji sadrži mnogo churn rezultirat će situaciji u kojoj možete brzo pokrenuti nema dovoljno prostora na lokalnom. Ako odaberete da bi lokalne snimke, preporučujemo da iskoristite manje dnevnih snimaka sigurnosno kopiranje najnovije stanje zadržati ih za dan, a zatim izbrišite ih.

Kada poduzmete snimku oblaka lokalno prikvačene jedinice, kopirajte samo promijenjene podatke u oblak, gdje je deduplicated i sažeti. 

## <a name="the-backup-policies-page"></a>Stranici sigurnosne kopije pravila

Stranici **Sigurnosne kopije pravila** omogućuje upravljanje pravilnicima za sigurnosne kopije i zakazivanje lokalne i brze snimke u oblaku. (Sigurnosne kopije pravila koriste za konfiguriranje sigurnosnih kopija rasporede i sigurnosne kopije zadržavanja za skup jedinicama) Sigurnosne kopije pravila omogućuju vam da stvorite snimku više jedinica istodobno. To znači da sigurnosne kopije stvorio sigurnosne kopije pravila će se srušiti dosljedan kopije. Stranica za **Sigurnosne kopije pravila** sadrži popis sigurnosne kopije pravila, njihove vrste, pridruženi količine, broj kopija zadržavaju i mogućnost da biste omogućili ta pravila.

Stranici **Sigurnosne kopije pravila** omogućuje vam da biste filtrirali postojeća sigurnosne kopije pravila tako da je jedan ili više sljedećih polja:

- **Naziv pravila** – ime pridruženo pravila. Različite vrste pravila obuhvaćaju sljedeće:

   - Zakazano pravila koja izričito stvorio korisnik.
   - Automatsko pravila, koje stvaraju kada zadane sigurnosne kopije za tu mogućnost glasnoću omogućivanja prilikom stvaranja glasnoću. Ta pravila imenovane su kao *VolumeName*_Default gdje *VolumeName* se odnosi na naziv glasnoće StorSimple konfigurirao korisnika na portalu za Azure klasični. Automatsko pravilnike rezultat dnevnih oblaka snimke početak trenutku uređaj 22:30.
   - Uvezene pravila, koji su izvorno stvoreni u upravitelju snimka StorSimple. Te su oznake koji opisuje voditelju StorSimple snimke Upravitelj pravila uvezenih iz.

- **Količine** – količine pridružene pravila. Sve jedinice pridružene sigurnosne kopije pravila grupiraju stvaranja sigurnosnih kopija.

- **Zadnje uspješno sigurnosno kopiranje** – datum i vrijeme zadnje uspješno sigurnosne kopije koju je snimljene ovo pravilo.

- **Sljedeće sigurnosno kopiranje** – datum i vrijeme na sljedeće zakazano sigurnosno kopiranje koji će se pokrenuo ovo pravilo.

- **Raspored** – broj rasporede pridružene sigurnosne kopije pravila.

Često korišteni operacije koje možete izvršiti s ove stranice su:

- Dodavanje sigurnosne kopije pravila 
- Dodavanje ili izmjena rasporeda 
- Brisanje sigurnosne kopije pravila 
- Iskoristite ručno sigurnosno kopiranje 
- Stvaranje prilagođene sigurnosne kopije pravila s više količine i rasporedima 

## <a name="add-a-backup-policy"></a>Dodavanje sigurnosne kopije pravila

Dodavanje sigurnosne kopije pravila za automatsko zakazivanje sigurnosne kopije. Na portalu Azure klasični da biste dodali sigurnosne kopije pravila za svoj uređaj StorSimple poduzeti sljedeće korake. Nakon što dodate pravilo, možete odrediti raspored (u odjeljku [Dodavanje ili izmjenu rasporeda](#add-or-modify-a-schedule)).

[AZURE.INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

![Videozapis dostupne](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **videozapis dostupna**

Da biste pogledali videozapis u kojem se pokazuje kako stvoriti na lokalni ili sigurnosne kopije pravila u oblaku, kliknite [ovdje](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).


## <a name="add-or-modify-a-schedule"></a>Dodavanje ili izmjena rasporeda

Možete dodati ili izmijeniti raspored koji je pridružen postojećeg sigurnosne kopije pravila na uređaju StorSimple. Na portalu Azure klasični za dodavanje ili promjenu rasporeda poduzeti sljedeće korake.

[AZURE.INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## <a name="delete-a-backup-policy"></a>Brisanje sigurnosne kopije pravila

Na portalu Azure klasični da biste izbrisali sigurnosne kopije pravila na uređaju StorSimple poduzeti sljedeće korake.

[AZURE.INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]


## <a name="take-a-manual-backup"></a>Iskoristite ručno sigurnosno kopiranje

Na portalu Azure klasični da biste stvorili na zahtjev (ručno) sigurnosnu za jednu poduzeti sljedeće korake.

[AZURE.INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Stvaranje prilagođene sigurnosne kopije pravila s više količine i rasporedima

Na portalu Azure klasični da biste stvorili prilagođeni sigurnosne kopije pravila koja sadrži više količine i rasporede poduzeti sljedeće korake.

[AZURE.INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]


## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [korištenju StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).
