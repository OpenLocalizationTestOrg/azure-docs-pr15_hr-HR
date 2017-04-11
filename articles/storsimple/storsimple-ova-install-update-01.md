<properties 
   pageTitle="Instalacija ažuriranja na polja za virtualne StorSimple | Microsoft Azure"
   description="Opisuje način korištenja na webu StorSimple virtualne polja korisničkog Sučelja da biste primijenili ažuriranja metodom portal i hitni popravak"
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
   ms.date="09/07/2016"
   ms.author="alkohli" />

# <a name="install-updates-on-your-storsimple-virtual-array"></a>Instalacija ažuriranja na vašem StorSimple virtualne polja

## <a name="overview"></a>Pregled

U ovom se članku opisuju koraci potrebni za instaliranje ažuriranja na vašem StorSimple virtualne polja putem lokalnog web korisničkog Sučelja i putem portala za Azure klasični. Morate primijeniti softverska ažuriranja ili hitnih za vaše StorSimple virtualne polja držite ažurnima. 

Imajte na umu da instalirate ažuriranje ili hitni popravak ponovnog pokretanja uređaja. Given da je argument polje za virtualne StorSimple jedan čvor uređaja, sve/i u tijeku je nadziranje prekinuto i uređaju iskustvo isključiti. 

Primjena ažuriranja, preporučujemo da prije da se snimljene količine ili zajedničko korištenje u izvanmrežnom načinu rada na računalu koje hostira prvi, a zatim željeni uređaj. Time se smanjuje bilo koju mogućnost oštećenja podataka.

> [AZURE.IMPORTANT] Ako su pokrenuti ažuriranje 0,1 ili GA verzija softvera, instalirajte ažuriranje 0,3 morate koristiti metodu hitni popravak putem lokalnog web korisničkog Sučelja. Ako koristite ažuriranje 0,2, preporučujemo da instalirate ažuriranja putem portala za Azure klasični.

## <a name="use-the-local-web-ui"></a>Korištenje lokalnog web korisničkog Sučelja 
 
Dva su koraka prilikom korištenja lokalnog web korisničkog Sučelja:

- Preuzmite ažuriranje ili hitni popravak
- Instalirajte ažuriranje ili hitni popravak

### <a name="download-the-update-or-the-hotfix"></a>Preuzmite ažuriranje ili hitni popravak

Izvršite sljedeće korake da biste preuzeli ažuriranje softvera Microsoft ažuriranje kataloga.

#### <a name="to-download-the-update-or-the-hotfix"></a>Da biste preuzeli ažuriranje ili hitni popravak

1. Pokretanje preglednika Internet Explorer, a zatim otvorite [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Ako je ovo prvi put pomoću kataloga za Microsoft Update na ovom računalu, kliknite **instalirajte** kada se to od vas zatraži da biste instalirali dodatak Microsoft Update kataloga.
  
3. U okvir za pretraživanje kataloga Microsoft Update unesite broj znanja (KB) hitni popravak koje želite preuzeti. Unesite **3182061** za ažuriranje 0,3, a zatim kliknite **pretraživanje**.

    Unos hitni popravak pojavljuje se ako, na primjer, **StorSimple virtualne polja ažuriranje 0,3**.

    ![Pretraživanje kataloga](./media/storsimple-ova-install-update-01/download1.png)

4. Kliknite **Dodaj**. Ažuriranje se dodaju u košaricu.

5. Kliknite **Prikaz košaricu**.

6. Kliknite **Preuzmi**. Navedite ili **Pronađite** lokalno mjesto gdje želite da se preuzimanja da se pojavi. Ažuriranja se preuzimaju na određeno mjesto i smještene u podmapu s istim nazivom kao i ažuriranje. Mapa je moguće kopirati i na zajednički mrežni resurs koji je dostupan putem uređaja.

7. Otvorite mapu kopirane, trebali biste vidjeti datoteku paketa programa Microsoft Update samostalne `WindowsTH-KB3011067-x64`. Datoteka koristi se za instaliranje ažuriranje ili hitni popravak.


### <a name="install-the-update-or-the-hotfix"></a>Instalirajte ažuriranje ili hitni popravak

Prije instalacije ažuriranje ili hitni popravak, provjerite je li da imate ažuriranje ili hitni popravak preuzeti bilo lokalno na vašem računalu koje hostira i pristupiti putem zajedničkog mrežnog resursa. 

Ovaj postupak koristite da biste na uređaju pokrenut GA instalirajte ažuriranja i ažuriranje 0,1 verzija softvera. Ovaj vas postupak vodi manje od dvije minute. Izvršite sljedeće korake da biste instalirali ažuriranje ili hitni popravak.


#### <a name="to-install-the-update-or-the-hotfix"></a>Da biste instalirali ažuriranje ili hitni popravak

1. U lokalnoj web korisničkog Sučelja idite na **Održavanje** > **Ažuriranje softvera**.

    ![Ažuriranje uređaja](./media/storsimple-ova-install-update-01/update1m.png)

2. U **Ažuriranje put datoteke**, unesite naziv datoteke za ažuriranje ili hitni popravak. Možete i pregledavati instalacijsku datoteku ažuriranje ili hitni popravak Ako stavite na zajednički mrežni resurs. Kliknite **Primijeni**.

    ![Ažuriranje uređaja](./media/storsimple-ova-install-update-01/update2m.png)

3.  Prikazat će se upozorenje. Navedeni to je jedan čvor uređaj, nakon primjene ažuriranja, ponovnog pokretanja uređaja, a postoji nedostupnost. Kliknite ikonu provjeri.

    ![Ažuriranje uređaja](./media/storsimple-ova-install-update-01/update3m.png)

4. Pokreće se ažuriranje. Kada uspješno ažurira uređaj, ponovnog pokretanja. Lokalni korisničko Sučelje nije dostupna u ovom trajanje.

    ![Ažuriranje uređaja](./media/storsimple-ova-install-update-01/update5m.png)

5. Po dovršetku ponovno pokretanje prijeći ćete na stranici **Prijava** . Da biste provjerili je li softver uređaja ažurirao, na lokalnom web-mjestu korisničko Sučelje, idite na **Održavanje** > **Ažuriranje softvera**. Verzija prikazane softvera mora biti **10.0.0.0.0.10288.0** za ažuriranje 0,3.

    > [AZURE.NOTE] Ne možemo izvješće verzija softvera u malo drugačije na lokalnom web-mjestu korisničkog Sučelja i Azure klasični portal. Ako, na primjer, lokalni web korisničkog Sučelja izvješća **10.0.0.0.0.10288** Azure klasični portal izvješćima i **10.0.10288.0** za istu verziju. 

    ![Ažuriranje uređaja](./media/storsimple-ova-install-update-01/update6m.png)





## <a name="use-the-azure-classic-portal"></a>Pomoću portala za Azure klasični

Ako je pokrenut ažuriranje 0,2, preporučujemo da instalirate ažuriranja putem portala za Azure klasični. Postupak portala zahtijeva od korisnika da skeniranje, preuzmite i instalirajte ažuriranja. Ovaj vas postupak vodi oko 7 minuta da biste dovršili. Izvršite sljedeće korake da biste instalirali ažuriranje ili hitni popravak.

[AZURE.INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

Nakon dovršetka instalacije (kako je naznačeno prema statusu zadatka na 100%), prijeđite na **uređaje > održavanje > softverska ažuriranja**. Verzija prikazane softvera mora biti 10.0.10288.0.

![Ažuriranje uređaja](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [administraciji sustava StorSimple virtualne polja](storsimple-ova-web-ui-admin.md).
