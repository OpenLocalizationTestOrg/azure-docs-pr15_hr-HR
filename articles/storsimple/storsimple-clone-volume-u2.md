<properties
   pageTitle="Kloniraj glasnoću StorSimple | Microsoft Azure"
   description="Opisuje Kloniraj različite vrste i kada ih koristiti, a u članku se objašnjava kako možete koristiti sigurnosnu kopiju postavljena na Kloniraj pojedinačne glasnoću."
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
   ms.date="07/27/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>Pomoću servisa StorSimple Upravitelj Kloniraj glasnoću (ažuriranje 2)

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Pregled

Stranica za **Katalog** StorSimple Upravitelj servisa prikazuje sve skupove sigurnosne kopije stvorene kada uzimaju se ručno i automatsko sigurnosno kopiranje. Koristite ovu stranicu da biste popis svih sigurnosnih kopija za sigurnosne kopije pravila ili količine, odaberite ili brisanje sigurnosne kopije ili koristite sigurnosnu kopiju da biste vratili ili Kloniraj jedinicu.

![Katalog stranice](./media/storsimple-clone-volume-u2/backupCatalog.png)  

Pomoću ovog praktičnog vodiča opisuje kako možete koristiti sigurnosnu kopiju postavljena na Kloniraj pojedinačne glasnoću. Objašnjava se i razlika između *tranzitne* i *Trajna* clones.

>[AZURE.NOTE] 
>
>Lokalno prikvačene glasnoću će biti klonirana kao tiered jedinica. Ako vam je potrebna kloniranu jedinice lokalno prikvačene, možete pretvoriti u Kloniraj lokalno prikvačene glasnoću po dovršetku postupka Kloniraj uspješno. Informacije o pretvaranju tiered glasnoću lokalno prikvačene jedinicu, prijeđite na odjeljak [Promjena vrste glasnoću](storsimple-manage-volumes-u2.md#change-the-volume-type).
>
>Ako pokušate jedinicu pretvoriti u kloniranu iz tiered za lokalno prikvačene odmah nakon kloniranje (kada je i dalje tranzitne Kloniraj), pretvorba neće uspjeti uz sljedeću poruku o pogrešci:
>
>`Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
>
>Ta se pogreška se prima samo ako su kloniranje na neki drugi uređaj. Uspješno možete pretvoriti jedinice lokalno prikvačene ako najprije pretvorite tranzitne Kloniraj trajna Kloniraj. Da biste pretvorili tranzitne Kloniraj trajna Kloniraj, stvorite snimku oblaka ga.

## <a name="create-a-clone-of-a-volume"></a>Stvaranje Kloniraj jedinice

Možete stvoriti na Kloniraj na istom uređaju, neki drugi uređaj ili čak i virtualnog računala pomoću na lokalni ili brze snimke oblaka.

#### <a name="to-clone-a-volume"></a>Da biste Kloniraj jedinica

1. Na stranici upravitelja StorSimple servisa kliknite karticu **kataloga sigurnosnu kopiju** , a zatim odaberite skup sigurnosne kopije.

2. Proširite skup da biste vidjeli povezane količine sigurnosnih kopija. Kliknite i odaberite jedinice iz skupa sigurnosne kopije.

     ![Kloniraj jedinica](./media/storsimple-clone-volume-u2/CloneVol.png) 

3. Kliknite **Kloniraj** da biste započeli kloniranje Odabrana jedinica.

4. U čarobnjaku Kloniraj jedinicu u odjeljku **Navedite naziv i mjesto**:

  1. Odredite ciljni uređaj. To je mjesto na kojem će se stvoriti u Kloniraj. Možete odabrati na istom uređaju ili odredite nekog drugog uređaja. Ako se odlučite glasnoću povezan s drugim davateljima usluga oblaka (ne Azure), na padajućem popisu za ciljni uređaj će prikazati samo fizičkih uređaja. Nije moguće Kloniraj glasnoću povezan s drugim davateljima usluga oblak na virtualnog uređaja.

        >[AZURE.NOTE] Provjerite je li kapacitet potreban za na Kloniraj manja od kapaciteta dostupne na ciljni uređaj.

  2. Navedite naziv jedinstveni glasnoću vaše Kloniraj. Naziv mora sadržavati od 3 do 127 znakova. 
    
        >[AZURE.NOTE] Polje **Kloniraj glasnoću kao** bit će **Tiered** čak i ako su kloniranje lokalno prikvačene glasnoću. Ne možete promijeniti ovu postavku; Međutim, ako vam je potrebna kloniranu jedinice lokalno i prikvačiti možete pretvoriti u Kloniraj lokalno prikvačene jedinicu kada stvorite uspješno na Kloniraj. Informacije o pretvaranju tiered glasnoću lokalno prikvačene jedinicu, prijeđite na odjeljak [Promjena vrste glasnoću](storsimple-manage-volumes-u2.md#change-the-volume-type).

        ![Čarobnjak za Kloniraj 1](./media/storsimple-clone-volume-u2/clone1.png) 

  3. Kliknite ikonu sa strelicom ![Ikona strelice](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) Da biste prešli na sljedeću stranicu.

5. U odjeljku **Navedite domaćini koje možete koristiti ovu jedinicu**:

  1. Navedite pristup kontrola zapis (ACR) za na Kloniraj. Možete dodati nove ACR ili odaberite postojeći popis.

        ![Čarobnjak za Kloniraj 2](./media/storsimple-clone-volume-u2/clone2.png) 

  2. Kliknite ikonu provjeri ![ikonu Provjera](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)Da biste dovršili postupak.

6. Kloniraj posao će se inicirati i bit ćete obaviješteni kada se Kloniraj uspješno stvorili. Kliknite **Prikaz posao** praćenje Kloniraj zadatka na stranici **Zadaci** . Nakon dovršetka posla Kloniraj će se pojaviti sljedeća poruka:

    ![Kloniraj poruke](./media/storsimple-clone-volume-u2/CloneMsg.png) 

7. Nakon završetka posla Kloniraj:

  1. Idite na stranicu **uređaji** pa odaberite karticu **Spremnika glasnoću** . 
  2. Odaberite kontejner glasnoću koji je pridružen glasnoće izvora koji ste klonirana. Na popisu količine, trebali biste vidjeti Kloniraj koji ste upravo stvorili.

>[AZURE.NOTE] Nadzor i zadane sigurnosne kopije su automatski onemogućeni na kloniranu jedinicu.

Kloniraj koja je stvorena na taj način je tranzitne Kloniraj. Dodatne informacije o vrstama Kloniraj, potražite u članku [tranzitne nasuprot trajna clones](#transient-vs.-permanent-clones).

U ovom Kloniraj je sada običnog glasnoće, a sve operacije koje je moguće jedinice će biti dostupna na Kloniraj. Morat ćete konfigurirati ove jedinice za sve sigurnosne kopije.

## <a name="transient-vs-permanent-clones"></a>Prolaznim nasuprot trajna clones

Tranzitne clones stvaraju se samo kada su kloniranje na drugi uređaj. Mogu Kloniraj određene jedinice iz sigurnosne kopije postavljena na neki drugi uređaj upravlja upravitelj StorSimple. Tranzitne Kloniraj neće sadržavati reference na podatke u izvornu glasnoću i će koristiti te podatke za čitanje i pisanje lokalno na uređaj cilj. 

Nakon što poduzmete snimku oblaka tranzitne Kloniraj, dobivene Kloniraj bit će *trajno* Kloniraj. Tijekom tog postupka stvorena je kopija podataka u oblak i vrijeme da biste kopirali podatke ovisi o veličini podatke i Azure latencies (to je na kopiju Azure Azure). Taj postupak može potrajati dana na tjedne. Tranzitne Kloniraj Pretvori trajno Kloniraj na taj način, a ne obuhvaća sve reference s izvornim podacima glasnoću koja ga je klonirana iz. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenariji za tranzitne i trajna clones

U sljedećim odjeljcima opisuju primjer situacije u kojima se može koristiti tranzitne i trajna clones.

### <a name="item-level-recovery-with-a-transient-clone"></a>Oporavak na razini stavke s tranzitne Kloniraj

Morate vratiti datoteke prezentacije jedan godina stari Microsoft PowerPoint. IT administrator prepoznaje određene sigurnosne kopije iz tog vremensko razdoblje i filtrira glasnoću. Administrator pa clones glasnoće, pronalazi datoteku koju tražite i omogućuje vam. U ovom scenariju tranzitne Kloniraj koristi. 
 
![Videozapis dostupne](./media/storsimple-clone-volume-u2/Video_icon.png) **videozapis dostupna**

Da biste pogledali videozapis u kojem se pokazuje kako možete koristiti u Kloniraj i vraćanje značajke u StorSimple da biste oporavili izbrisane datoteke, kliknite [ovdje](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Testiranje u okruženju proizvodnje s trajna Kloniraj

Morate da biste provjerili testiranja pogrešku u radnom okruženju. Stvaranje Kloniraj jedinice u okruženju radnog, a zatim stvorite snimku oblaka ovaj Kloniraj da biste stvorili neovisno kloniranu glasnoću. U ovom primjeru koristi se trajno Kloniraj.  

## <a name="next-steps"></a>Daljnji koraci
- Saznajte kako [vratiti StorSimple jedinice iz sigurnosne kopije skupa](storsimple-restore-from-backup-set-u2.md).

- Saznajte kako [koristiti StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).

 
