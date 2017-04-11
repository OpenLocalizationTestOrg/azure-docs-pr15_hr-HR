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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume"></a>Pomoću upravitelja StorSimple servisa Kloniraj jedinica

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Pregled

Stranica za **Katalog** StorSimple Upravitelj servisa prikazuje sve skupove sigurnosne kopije stvorene kada uzimaju se ručno i automatsko sigurnosno kopiranje. Koristite ovu stranicu da biste popis svih sigurnosnih kopija za sigurnosne kopije pravila ili količine, odaberite ili brisanje sigurnosne kopije ili koristite sigurnosnu kopiju da biste vratili ili Kloniraj jedinicu.

![Katalog stranice](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

Pomoću ovog praktičnog vodiča opisuje kako možete koristiti sigurnosnu kopiju postavljena na Kloniraj pojedinačne glasnoću. Objašnjava se i razlika između *tranzitne* i *Trajna* clones. 

## <a name="create-a-clone-of-a-volume"></a>Stvaranje Kloniraj jedinice

Možete stvoriti na Kloniraj na istom uređaju, neki drugi uređaj ili čak i virtualnog računala pomoću na lokalni ili snimku oblaka.

#### <a name="to-clone-a-volume"></a>Da biste Kloniraj jedinica

1. Na stranici upravitelja StorSimple servisa kliknite karticu **kataloga sigurnosnu kopiju** , a zatim odaberite skup sigurnosne kopije.

2. Proširite skup da biste vidjeli povezane količine sigurnosnih kopija. Kliknite i odaberite jedinice iz skupa sigurnosne kopije.

     ![Kloniraj jedinica](./media/storsimple-clone-volume/HCS_Clone.png) 

3. Kliknite **Kloniraj** da biste započeli kloniranje Odabrana jedinica.

4. U čarobnjaku Kloniraj jedinicu u odjeljku **Navedite naziv i mjesto**:

  1. Odredite ciljni uređaj. To je mjesto na kojem će se stvoriti u Kloniraj. Možete odabrati na istom uređaju ili odredite nekog drugog uređaja. Ako se odlučite glasnoću povezan s drugim davateljima usluga oblaka (ne Azure), padajućeg popisa za ciljni uređaj će prikazati samo fizičkih uređaja. Nije moguće Kloniraj glasnoću povezan s drugim davateljima usluga oblak na virtualnog uređaja.

        >  [AZURE.NOTE] Provjerite je li kapacitet potreban za na Kloniraj manja od kapaciteta dostupne na ciljni uređaj.
  2. Navedite naziv jedinstveni glasnoću vaše Kloniraj. Naziv mora sadržavati od 3 do 127 znakova.
  3. Kliknite ikonu sa strelicom ![Ikona strelice](./media/storsimple-clone-volume/HCS_ArrowIcon.png) Da biste prešli na sljedeću stranicu.

5. U odjeljku **Navedite domaćini koje možete koristiti ovu jedinicu**:

  1. Navedite pristup kontrola zapis (ACR) za na Kloniraj. Možete dodati nove ACR ili odaberite postojeći popis.
  2. Kliknite ikonu provjeri ![ikonu Provjera](./media/storsimple-clone-volume/HCS_CheckIcon.png)Da biste dovršili postupak.

6. Kloniraj posao će se inicirati i bit ćete obaviješteni kada se Kloniraj uspješno stvorili. Kliknite **Prikaz poslova** praćenje Kloniraj zadatka na stranici **Zadaci** .

7. Nakon završetka posla Kloniraj:

  1. Idite na stranicu **uređaji** pa odaberite karticu **Spremnika glasnoću** . 
  2. Odaberite kontejner glasnoću koji je pridružen glasnoće izvora koji ste klonirana. Na popisu količine, trebali biste vidjeti Kloniraj koji ste upravo stvorili.

>[AZURE.NOTE] Nadzor i zadane sigurnosne kopije su automatski onemogućeni na kloniranu jedinicu.

Kloniraj koja je stvorena na taj način je tranzitne Kloniraj. Dodatne informacije o vrstama Kloniraj potražite u članku [tranzitne nasuprot trajna clones](#transient-vs.-permanent-clones).

U ovom Kloniraj je sada običnog glasnoće, a sve operacije koje je moguće jedinice će biti dostupna na Kloniraj. Morat ćete konfigurirati ove jedinice za sve sigurnosne kopije.

## <a name="transient-vs-permanent-clones"></a>Prolaznim nasuprot trajna clones

Tranzitne i trajna clones stvaraju se samo kada su kloniranje na neki drugi uređaj. Mogu Kloniraj određene jedinice iz sigurnosne kopije postavljena na neki drugi uređaj. Kloniraj stvorena na taj način je *tranzitne* Kloniraj. Tranzitne Kloniraj sadržavat će reference na izvornu glasnoću, a da biste pročitali tijekom pisanja lokalno će koristiti tu jedinicu. 

Nakon što preuzeli snimku oblaka tranzitne Kloniraj, rezultirajući Kloniraj bit će *trajno* Kloniraj. Trajno Kloniraj neovisan je, a ne obuhvaća sve reference na izvornu glasnoću koja ga je klonirana iz.  

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenariji za tranzitne i trajna clones

U sljedećim odjeljcima opisuju primjer situacije u kojima se može koristiti tranzitne i trajna clones.

### <a name="item-level-recovery-with-a-transient-clone"></a>Oporavak na razini stavke s tranzitne Kloniraj

Morate vratiti datoteke prezentacije jedan godina stari Microsoft PowerPoint. IT administrator prepoznaje određene sigurnosne kopije iz tog vremensko razdoblje i filtrira glasnoću. Administrator pa clones glasnoće, pronalazi datoteku koju tražite i omogućuje vam. U ovom scenariju tranzitne Kloniraj koristi. 
 
![Videozapis dostupne](./media/storsimple-clone-volume/Video_icon.png) **videozapis dostupna**

Da biste pogledali videozapis u kojem se pokazuje kako možete koristiti u Kloniraj i vraćanje značajke u StorSimple da biste oporavili izbrisane datoteke, kliknite [ovdje](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Testiranje u okruženju proizvodnje s trajna Kloniraj

Morate da biste provjerili testiranja pogrešku u radnom okruženju. Stvorite Kloniraj jedinice u okruženju proizvodnje prihvaćanjem snimke oblak ovaj Kloniraj. Sada je nezavisne kloniranu glasnoću. U ovom scenariju trajne Kloniraj koristi.

## <a name="next-steps"></a>Daljnji koraci
- Saznajte kako [vratiti StorSimple jedinice iz sigurnosne kopije skupa](storsimple-restore-from-backup-set.md).

- Saznajte kako [pomoću upravitelja StorSimple servisa za administraciju uređaju StorSimple](storsimple-manager-service-administration.md).

 
