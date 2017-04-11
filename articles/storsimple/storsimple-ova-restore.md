<properties
   pageTitle="Vraćanje iz sigurnosne kopije vaše StorSimple virtualne polja"
   description="Dodatne informacije o vraćanju sigurnosnu kopiju vašeg StorSimple virtualne polja."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="restore-from-a-backup-of-your-storsimple-virtual-array"></a>Vraćanje iz sigurnosne kopije vaše StorSimple virtualne polja

## <a name="overview"></a>Pregled 

Ovaj se članak odnosi na Microsoft Azure StorSimple virtualne polja (poznat i kao StorSimple lokalnog virtualnog uređaja ili StorSimple virtualnog uređaja) izvodi ožujak 2016 Općenito dostupan (GA) izdanje ili noviju verziju. U ovom se članku opisuje detaljne vraćanje iz sigurnosne kopije skupa zajedničko korištenje ili količine na vašem StorSimple virtualne polja. U članku Detalji i funkcioniranje oporavak razini stavke na polja virtualne vaše StorSimple koji je konfiguriran kao datotečnom poslužitelju.


## <a name="restore-shares-from-a-backup-set"></a>Vraćanje dionice iz skupa sigurnosne kopije


**Prije no što pokušate vratiti dionice, provjerite imate li dovoljno prostora na uređaju da biste dovršili postupak.** Vraćanje iz sigurnosne kopije, [Azure klasični portal](https://manage.windowsazure.com/)poduzeti sljedeće korake.

#### <a name="to-restore-a-share"></a>Da biste vratili zajedničke mape

1.  Pronađite **sigurnosnu kopiju kataloga**. Filtriranje prema odgovarajući uređaj i vremenski raspon da biste potražili sigurnosne kopije. Kliknite ikonu provjeri ![](./media/storsimple-ova-restore/image1.png) za izvršavanje upita.


1.  Na popisu sigurnosne kopije skupove prikazuje kliknite i odaberite određene sigurnosnu kopiju. Proširite sigurnosnu kopiju da biste vidjeli različite dionice ispod nje. Kliknite, a zatim odaberite zajedničko korištenje koji želite vratiti.

2.  Pri dnu stranice kliknite **Vrati kao nove**.

3.  To će pokrenuti čarobnjak za **Vraćanje kao novi zajedničko korištenje** . Na stranici **Navedite naziv i mjesto** :


    1.  Provjerite je li uređaj naziv izvora. Trebali biste uređaj koji sadrži zajedničko korištenje želite vratiti. Odabir uređaja je zasivljena. Da biste odabrali neki drugi izvor uređaj, morat ćete zatvoriti čarobnjak i ponovno odaberite ponovno skupa sigurnosnih kopija.

    2.  Navedite naziv zajedničko korištenje. Zajedničko korištenje naziv mora sadržavati 3 do 127.

    3.  Pregledajte veličinu, vrstu i dozvole povezane s zajedničko korištenje koji želite vratiti. Moći da biste izmijenili svojstva zajedničko korištenje putem programa Windows Explorer po dovršetku vraćanja.

    4.  Kliknite ikonu provjeri ![](./media/storsimple-ova-restore/image1.png).

        ![](./media/storsimple-ova-restore/image9.png)

1.  Po dovršetku vraćanja posao vraćanja će se pokrenuti i vidjet ćete drugi obavijesti. Da biste pratili tijek vraćanja, kliknite **Prikaz zadatka**. To će vas odvesti stranici **Poslovi** .

2.  Možete pratiti napredak vraćanja posla. Kada je Vrati 100% dovršeno, idite natrag na stranici **zajedničko korištenje** na uređaju.

3.  Sada možete vidjeti novi vraćene zajedničko korištenje na popisu zajedničko korištenje na uređaju. Imajte na umu te vraćanja Završi da biste iste vrste koji se zajednički koristi. Kao što je tiered, vratit će se tiered zajedničko korištenje i zajedničko korištenje lokalno prikvačene kao lokalno prikvačene zajedničko korištenje.

Sada dovršite Konfiguracija uređaja i naučili kako sigurnosno kopiranje i vraćanje na zajedničko korištenje. 


## <a name="restore-volumes-from-a-backup-set"></a>Vraćanje količine iz skupa sigurnosne kopije


Vraćanje iz sigurnosne kopije, Azure klasični portalu poduzeti sljedeće korake. Postupak vraćanja vraća sigurnosne kopije na novu jedinicu na istom uređaju virtualne; ne možete se vratiti na drugi uređaj.

#### <a name="to-restore-a-volume"></a>Da biste vratili jedinica

1.  Pronađite **sigurnosnu kopiju kataloga**. Filtriranje prema odgovarajući uređaj i vremenski raspon da biste potražili sigurnosne kopije. Kliknite ikonu provjeri ![](./media/storsimple-ova-restore/image1.png) za izvršavanje upita.

2.  Na popisu sigurnosne kopije skupove prikazuje kliknite i odaberite određene sigurnosnu kopiju. Proširite sigurnosnu kopiju da biste vidjeli različite količine ispod nje. Odaberite jedinicu koju želite vratiti. 

5.  Pri dnu stranice kliknite **Vrati kao nove**. Čarobnjak za **Vraćanje kao novi jedinica** započet će.

1.  Na stranici **Navedite naziv i mjesto** :


    1.  Provjerite je li uređaj naziv izvora. Trebali biste uređaj koji sadrži jedinicu koju želite vratiti. Odabir uređaja nije dostupna. Da biste odabrali neki drugi izvor uređaj, morat ćete zatvoriti čarobnjak i ponovno odaberite ponovno skupa sigurnosnih kopija.

    2.  Navedite naziv jedinica za jedinicu kao nove vratiti. Naziv glasnoću mora sadržavati 3 do 127.

    3.  Kliknite ikonu sa strelicom.

        ![](./media/storsimple-ova-restore/image12.png)

1.  Na stranici **Određivanje domaćini koje možete koristiti ovu jedinicu** odaberite odgovarajući ACRs s padajućeg popisa.

    ![](./media/storsimple-ova-restore/image13.png)

1.  Kliknite ikonu provjeri ![](./media/storsimple-ova-restore/image1.png). To će pokrenuti zadatak obnavljanja i vidjet ćete sljedeću obavijest koja je zadatak u tijeku.

2.  Po dovršetku vraćanja posao vraćanja će se pokrenuti i vidjet ćete drugi obavijesti. Da biste pratili tijek vraćanja, kliknite **Prikaz zadatka**. To će vas odvesti stranici **Poslovi** .

3.  Možete pratiti napredak zadatak obnavljanja. Otvorite stranicu **količine** na uređaju.

4.  Sada možete vidjeti nova vraćene jedinica na popisu količine na uređaju. Imajte na umu te vraćanja Završi da biste je ista vrsta glasnoću. Kao što je tiered, vratit će se tiered glasnoću i lokalno prikvačene glasnoću vratit će se kao lokalne prikvačene jedinica.

5.  Kada glasnoću pojavit će se mreži na popisu jedinicama Glasnoća je dostupan za korištenje.  Na glavnom računalu pokretač iSCSI, osvježite popis ciljnih web-mjesta u prozoru svojstva pokretač iSCSI.  Novi cilj koji sadrži naziv vraćene glasnoću prikazivati kao 'neaktivan"u stupcu stanje.

6.  Odaberite ciljnu datoteku, a zatim kliknite **Poveži**.   Kada je pokretač cilj, status trebali biste promijeniti **povezan**. 

7.  U prozoru **Disk Management** postavljenu količine pojavit će se kao što je prikazano na sljedećoj slici. Desnom tipkom miša kliknite otkriven glasnoću (kliknite naziv diska), a zatim kliknite na **mreži**.

> [AZURE.IMPORTANT] Prilikom pokušaja vraćanje jedinice ili zajedničke mape iz sigurnosne kopije postavite, ako Vrati ne uspije, Ciljna jedinica ili zajedničko korištenje i dalje mogu se kreirati na portalu. Nije važno brisanje ciljne jedinicu ili zajedničko korištenje na portalu za minimiziranje buduće probleme nastale taj element.

## <a name="item-level-recovery-ilr"></a>Oporavak na razini stavke (ILR)

U ovom izdanju predstavlja oporavak razini stavke (ILR) na uređaju virtualne StorSimple konfigurirati kao datotečnom poslužitelju. Značajka omogućuje stvoriti zrnastog oporavak datoteka i mapa u oblak sigurnosnu kopiju dionice na uređaju StorSimple. Korisnicima možete dohvatiti izbrisane datoteke iz zadnje sigurnosne kopije pomoću samoposlužnog modela.

Svaki zajedničko korištenje ima *.backups* mapu koja sadrži zadnje sigurnosne kopije. Korisnik možete do željenog sigurnosnog kopiranja, kopirajte relevantne datoteke i mape iz sigurnosne kopije i njihova vraćanja. Uklanja se poziva administratorima na za vraćanje datoteka iz sigurnosne kopije.

1.  Prilikom izvršavanja u ILR, možete pogledati sigurnosne kopije pomoću programa Windows Explorer. Kliknite zajedničko korištenje određene koji želite pogledati sigurnosne kopije za. Prikazat će se mape *.backups* stvorena u odjeljku zajednički koristi u kojoj su pohranjeni sve sigurnosne kopije. Proširite mapu *.backups* da biste pogledali sigurnosnih kopija. Mapa vode će exploded prikaz hijerarhije cijelu sigurnosno kopiranje. Tom prikazu stvorena na zahtjev i obično traje samo nekoliko sekundi da biste stvorili.

    Zadnje 5 sigurnosne kopije prikazuju se na taj način, a mogu se izvoditi na razini stavke oporavak. 5 zadnje sigurnosne kopije obuhvaćaju zadani zakazano i ručno sigurnosno kopiranje.

    
    -   **Sigurnosno kopiranje zakazano** imenovan kao &lt;naziv uređaja&gt;DailySchedule-GGGGMMDD-HHMMSS-UTC-a.

    -   **Ručno sigurnosno kopiranje** imenovan kao Ad-hoc-GGGGMMDD-HHMMSS-UTC-a.
    
        ![](./media/storsimple-ova-restore/image14.png)

1.  Odredite koja sadrži najnoviju verziju izbrisane datoteke sigurnosne kopije. Iako naziv mape sadrži UTC vremenske oznake u svakoj iznad slučajeva, stvarni uređaj vrijeme kada je počeo sigurnosne kopije je vrijeme stvoriti mapu. Pomoću vremenske oznake mape da biste pronašli i prepoznavanje sigurnosnih kopija.

2.  Pronađite mapu ili datoteku koju želite vratiti u sigurnosne kopije koju ste naveli u prethodnom koraku. Možete vidjeti samo datoteke ili mape koje imate dozvole za bilješke. Ako se ne mogu pristupiti određene datoteke ili mape, morat ćete obratite se administratoru za zajedničko korištenje koji možete koristiti Windows Explorer za uređivanje dozvola za zajedničko korištenje i dati pristup određene datoteke ili mape. Je najbolje preporučljivo da se administrator za zajedničko korištenje korisnika grupe umjesto jednog korisnika.

3.  Kopirajte datoteku ili mapu odgovarajuće zajedničko korištenje na poslužitelj datoteka StorSimple.

![video_icon](./media/storsimple-ova-restore/video_icon.png) **videozapis dostupna**

Pogledajte videozapis da biste saznali kako možete stvoriti dionice, sigurnosno kopiranje dionice i vraćanje podataka na polja za virtualne StorSimple.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Daljnji koraci

Saznajte više o tome kako [upravljati vaše StorSimple polja virtualne putem lokalnog weba korisničkog Sučelja](storsimple-ova-web-ui-admin.md).
