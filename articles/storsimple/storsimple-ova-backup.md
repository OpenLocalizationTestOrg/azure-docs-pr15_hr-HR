<properties 
   pageTitle="Vodič za sigurnosno kopiranje StorSimple virtualne polja | Microsoft Azure"
   description="U članku se opisuje sigurnosno dionice StorSimple virtualne polja i količine."
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
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="back-up-your-storsimple-virtual-array"></a>Stvaranje sigurnosne kopije vaše StorSimple virtualne polja

## <a name="overview"></a>Pregled 

Pomoću ovog praktičnog vodiča odnosi na Microsoft Azure StorSimple virtualne polja (poznat i kao StorSimple lokalnog virtualnog uređaja ili StorSimple virtualnog uređaja) izvodi ožujak 2016 Općenito dostupan (GA) izdanje ili novije verzije.

Polja virtualne StorSimple je hibridnog oblaka prostora za pohranu lokalnog virtualne uređaj koji se može konfigurirati kao poslužitelj datoteka ili poslužitelju komponente iSCSI. Je stvaranje sigurnosne kopije, vraćanje iz sigurnosne kopije i izvođenje prebacivanje uređaja ako je potrebno Izrada oporavak. Kada je konfiguriran kao datotečnom poslužitelju, omogućuje oporavak razini stavke. Pomoću ovog praktičnog vodiča opisuje način korištenja portala za Azure klasični ili na webu StorSimple korisničkog Sučelja da biste stvorili zakazano i ručno sigurnosne kopije vaše StorSimple virtualne polja.


## <a name="back-up-shares-and-volumes"></a>Stvaranje sigurnosne kopije dionice i količine

Sigurnosno kopiranje zaštiti točke u vrijeme, poboljšati mogućnost oporavka i minimiziranje i vraćanje vremena za zajedničko korištenje i količine. Možete stvoriti sigurnosnu zajedničko korištenje ili količine na uređaju StorSimple na dva načina: **zakazano** ili **ručno**. Svaki od metoda opisan u sljedećim odjeljcima.

> [AZURE.NOTE] U ovom izdanju zakazanog sigurnosne kopije stvaraju zadani pravilnik koji se pokreće svaki dan u određeno vrijeme i sigurnosno kopira sve zajedničke stavke i količine na uređaju. Nije moguće stvoriti prilagođeni pravila za zakazano sigurnosne kopije trenutno.

## <a name="change-the-backup-schedule"></a>Promijenite raspored sigurnosnog kopiranja

Vaš uređaj virtualne StorSimple ima zadane sigurnosne kopije pravila koja pokreće u određeno vrijeme dana (22:30) i stvara sigurnosnu kopiju zajedničke stavke i količine na uređaju jednom dnevno. Možete promijeniti vrijeme na kojoj se pokreće sigurnosne kopije, ali učestalost i zadržavanja (koji navodi broj kopija da biste zadržali) nije moguće promijeniti. Tijekom ove sigurnosne kopije, cijeli virtualnog uređaja se sigurnosno; Stoga preporučujemo da zakazivanje te sigurnosne kopije za sati slabog opterećenja Budući.

[Azure klasični portal](https://manage.windowsazure.com/) da biste promijenili zadane sigurnosne kopije pokretanja, izvršite sljedeće korake.

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>Da biste promijenili vrijeme početka zadane sigurnosne kopije pravila

1. Idite na karticu **Konfiguracija** uređaja.

2. U odjeljku **sigurnosne kopije** odredite vrijeme početka dnevna sigurnosna kopija.

3. Kliknite **Spremi**.

### <a name="take-a-manual-backup"></a>Iskoristite ručno sigurnosno kopiranje

Osim zakazano sigurnosne kopije, koje možete poduzeti ručno (zahtjev) sigurnosne kopije u bilo kojem trenutku.

#### <a name="to-create-a-manual-on-demand-backup"></a>Da biste stvorili ručno sigurnosno kopiranje (zahtjev)

1. Idite na karticu **zajedničko korištenje** ili karticu **količine** .

2. Pri dnu stranice kliknite **sigurnosno kopiranje sve**. Zatražit će se da biste potvrdili da biste željeli da biste odmah preuzeli sigurnosnu kopiju. Kliknite ikonu provjeri ![provjerite ikona](./media/storsimple-ova-backup/image3.png) da biste nastavili s sigurnosnu kopiju.

    ![sigurnosno kopiranje potvrdu](./media/storsimple-ova-backup/image4.png)

    Bit ćete obaviješteni da se pokreće sigurnosno kopiranje.

    ![sigurnosno kopiranje pokretanja](./media/storsimple-ova-backup/image5.png)

    Bit ćete obaviješteni uspješno stvorili posao.

    ![sigurnosno kopiranje stvorili](./media/storsimple-ova-backup/image7.png)

3. Da biste pratili tijek zadatka, kliknite **Prikaz zadatka**.

4. Nakon dovršetka zadatka sigurnosnog kopiranja idite na karticu **kataloga sigurnosnu kopiju** . Trebali biste vidjeti dovršene sigurnosnu kopiju.

    ![Stvaranje sigurnosne](./media/storsimple-ova-backup/image8.png)

5. Postavljanje mogućnosti filtra odgovarajući uređaj, sigurnosne kopije pravila i vremenski raspon, a zatim kliknite ikonu Provjera ![Provjera ikona](./media/storsimple-ova-backup/image3.png).

    Sigurnosno kopiranje prikazivati na popisu sigurnosne kopije skupovi koji se prikazuje u katalogu.

## <a name="view-existing-backups"></a>Prikaz postojeće sigurnosne kopije

Na portalu Azure klasični prikaz postojeće sigurnosne kopije poduzeti sljedeće korake.

#### <a name="to-view-existing-backups"></a>Da biste pogledali postojeće sigurnosne kopije

1. Na stranici servisa StorSimple Upravitelj karticu **kataloga sigurnosnu kopiju** .

2. Odaberite sigurnosne kopije postavite na sljedeći način:

    1. Odaberite uređaj.

    2. Na padajućem popisu odaberite zajedničko korištenje ili jedinice za sigurnosne kopije koju želite odabrati.

    3. Odredite raspon vremena.

    4. Kliknite ikonu provjeri ![](./media/storsimple-ova-backup/image3.png) za izvršavanje ovog upita.

    Sigurnosnih kopija povezan s odabranog zajedničko korištenje ili glasnoću prikazivati na popisu skupa sigurnosne kopije.

![video_icon](./media/storsimple-ova-backup/video_icon.png) **videozapis dostupna**

Pogledajte videozapis da biste saznali kako možete stvoriti dionice, sigurnosno kopiranje dionice i vraćanje podataka na polja za virtualne StorSimple.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [administraciji sustava StorSimple virtualne polja](storsimple-ova-web-ui-admin.md).
