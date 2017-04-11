<properties 
   pageTitle="Upravitelj snimka StorSimple sigurnosne kopije zadataka | Microsoft Azure"
   description="U članku se opisuje kako koristiti dodatak za BLOG StorSimple snimke upravitelja za prikaz i upravljanje zakazani pokrenutim i dovršene sigurnosne kopije zadataka."
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


# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>Korištenje StorSimple snimke upravitelja za prikaz i upravljanje zadacima sigurnosne kopije

## <a name="overview"></a>Pregled

Čvor **zadacima** u oknu **opseg** prikazuje **zakazano**, **posljednje 24 sata**i **pokrenut** sigurnosne kopije zadataka koje ste pokrenuli interaktivno ili konfigurirani pravila. 

Pomoću ovog praktičnog vodiča objašnjava kako možete koristiti čvor **poslova** za prikaz informacija o zakazano, nedavno i pokrenutim poslova stvaranja sigurnosne kopije. (Popis zadataka i odgovarajuće podatke pojavit će se u oknu s **rezultatima** .) Osim toga, možete desnom tipkom miša kliknite navedenih posla i vidjeti Kontekstni izbornik s popisom dostupne akcije.

## <a name="view-scheduled-jobs"></a>Prikaz zakazano poslova

Koristite sljedeći postupak da biste pogledali Zakazani zadaci sigurnosne kopije.

#### <a name="to-view-scheduled-jobs"></a>Da biste pogledali Zakazani zadaci

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini. 

2. U oknu **opseg** Proširite čvor **zadataka** pa kliknite **zakazano**. U oknu **rezultata** prikazuje se sljedeće informacije:

    - **Naziv** – naziv raspoređene snimke

    - **Sljedeći pokrenuti** – datum i vrijeme sljedećeg raspoređene snimke

    - **Zadnje izvođenje** – datum i vrijeme zadnje raspoređene snimke

    >[AZURE.NOTE] Za jednokratni samo snimke, **Pokrenite sljedeću** i **Zadnje izvođenje** će biti iste. 
 
    ![Zakazano sigurnosne kopije zadataka](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
 
3. Da biste dodatne akcije na određeni posao, desnom tipkom miša kliknite naziv zadatka u oknu s **rezultatima** , a zatim odaberite jednu od mogućnosti izbornika.

## <a name="view-recent-jobs"></a>Prikaz nedavnih poslova

Pomoću sljedećeg postupka stvaranja i vraćanja zadataka koji su dovršeni u zadnja 24 sata.

#### <a name="to-view-recent-jobs"></a>Da biste vidjeli nedavne poslova

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

2. U oknu **opseg** Proširite čvor **zadataka** pa kliknite **posljednji 24 sata**. Okno s **rezultatima** prikazuje sigurnosne kopije zadataka za zadnja 24 sata (za najviše 64 zadaci). U oknu **rezultata** , ovisno o mogućnosti **prikaza** odredite pojavit će se sljedeće informacije:

    - **Naziv** – naziv raspoređene snimke.
 
    - Početak **rada** – datum i vrijeme kada je počeli snimke.

    - **Zaustavljen** – datum i vrijeme kada završite s snimku ili prekinut.

    - **Proteklo** – vremenskog razdoblja između **rada** i **zaustavljanja** vrijeme.

    - **Status** – stanje nedavno dovršeni zadatak. **Uspjeh** označava uspješno stvorili sigurnosnu kopiju. **Nije uspjelo** upućuje na to da posao nije uspješno izveden.

    - **Informacije o** – razlog neuspjeha.

    - **Bajtova obrađuju (MB)** – količine podataka iz grupe jedinica koji nije obrađen (u MB prostora). 

    ![Zadaci koje je pokrenuta u zadnja 24 sata](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 

3. Da biste dodatne akcije na određeni posao, desnom tipkom miša kliknite naziv zadatka u oknu s **rezultatima** , a zatim odaberite jednu od mogućnosti izbornika.

    ![Brisanje posla](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png) 
     
## <a name="view-currently-running-jobs"></a>Prikaz zadataka u tijeku

Pomoću sljedećeg postupka za prikaz poslova koji su trenutno pokrenuti.

#### <a name="to-view-currently-running-jobs"></a>Da biste pogledali pokrenutim poslove

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

2. U oknu **opseg** Proširite čvor **zadataka** pa kliknite **pokrenut**. Ovisno o mogućnosti **prikaza** koje ste odredili, u oknu s **rezultatima** pojavit će se sljedeće informacije: 

    - **Naziv** – naziv raspoređene snimke.

    - Početak **rada** – datum i vrijeme kada je počeli snimke.

    - **Kontrolna točka** – trenutne akcije sigurnosnu kopiju.

    - **Status** – postotak dovršenog.
    
    - **Proteklo** – vrijeme proteklo jer je počeo sigurnosnu kopiju. 

    - **Prosječna propusnost (MB)** – omjer Ukupno bajtova podataka obrađuje na Ukupno vrijeme za obradu (MB prostora).

    - **Bajtova obrađuju (MB)** – Ukupno bajtova podataka obrađuje (u MB prostora).

    - **Bajtova napisali (MB)** – Ukupno bajtova podataka pisane (MB prostora). Obuhvaća podatke, kao i metapodaci i zato je obično veći od obrađuju bajtova.

    ![Zadaci u tijeku](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)

3. Da biste dodatne akcije na određeni posao, desnom tipkom miša kliknite naziv zadatka u oknu s **rezultatima** , a zatim odaberite jednu od mogućnosti izbornika.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [pomoću upravitelja snimka StorSimple za administriranje StorSimple rješenje](storsimple-snapshot-manager-admin.md).
- Saznajte kako [pomoću upravitelja snimka StorSimple da biste upravljali katalog sigurnosne kopije](storsimple-snapshot-manager-manage-backup-catalog.md).















            


 

