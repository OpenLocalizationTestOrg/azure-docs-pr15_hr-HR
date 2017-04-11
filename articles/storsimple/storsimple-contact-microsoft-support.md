<properties 
   pageTitle="Obratite se Microsoftovoj podršci | Microsoft Azure"
   description="Saznajte kako stvoriti zahtjev za podršku i stvaranje sesije podršci na uređaju StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="contact-microsoft-support"></a>Obratite se Microsoftovoj podršci

Ako naiđete na probleme s vašeg rješenja za Microsoft Azure StorSimple, možete stvoriti zahtjev za uslugu za tehničku podršku. U sesiju online s inženjer za podršku možda potrebno da biste započeli sesiju za podršku na uređaju StorSimple. U ovom se članku objašnjavaju:

- Kako stvoriti zahtjev za podršku.
- Upute za stvaranje sesije podrška u sučelju programa Windows PowerShell StorSimple uređaja.

Pregledajte [StorSimple 8000 niz podršku SLA i informacije](https://msdn.microsoft.com/library/mt433077.aspx) prije nego što stvorite zahtjev za podršku.

## <a name="create-a-support-request"></a>Stvaranje zahtjeva za podršku

Poduzmite sljedeće korake da biste stvorili zahtjev za podršku:

#### <a name="to-create-a-support-request"></a>Da biste stvorili zahtjev za podršku

1. [Azure klasični portal](https://manage.windowsazure.com/)u gornjem desnom kutu kliknite naziv računa, a zatim **Kontakt Microsoftovoj podršci**.

    ![Podrška za MS kontakt putem ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)

2. Bit ćete preusmjereni na novi Azure portal (portal.azure.com). Kliknite pločicu **Novi zahtjev za podršku** .

    ![Podrška za MS kontakt putem novi portal](./media/storsimple-contact-microsoft-support/Ibiza2.png)

    Na desnoj strani zaslona, pojavit će se okno **Novi zahtjev za podršku** . 

    ![Novo okno zahtjev za podršku](./media/storsimple-contact-microsoft-support/Ibiza3a.png)

3. U dijaloškom okviru **Osnove** ispunite sljedeće:                                
    1. Na padajućem popisu **Vrsta problema** odaberite **tehničke**.
    2. Odaberite **pretplatu** iz padajućeg popisa.
    3. Na padajućem popisu **servisa** odaberite **StorSimple**. 
    4. S padajućeg popisa odaberite **plan za podršku** . Potreban vam je plan plaćena podrška da biste omogućili tehničke podrške.

4. Kliknite **Dalje**. Pojavit će se dijaloški okvir **Problem** .

    ![Novo okno zahtjev za podršku](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 

5. U dijaloškom okviru **problema** unesite sljedeće:

    1.  Na padajućem popisu odaberite razinu **težinu** .
    2.  Odaberite **vrstu Problem** s padajućeg popisa.
    3.  Na padajućem popisu odaberite **kategoriju** . 
    4.  U okviru **Detalji** ukratko opisuje problem.
    5.  U okvir **vremensko razdoblje** označiti datum, vrijeme i vremensku zonu koja odgovara najnovije pojavljivanja problem.
    6.  U odjeljku **Prijenos datoteke**, kliknite ikonu mape da biste pronašli paketa za podršku.
    7.  Potvrdite okvir **dijagnostičke informacije za zajedničko korištenje** .

6. Kliknite **Dalje**. Pojavit će se dijaloški okvir **Podaci za kontakt** .

    ![Novo okno zahtjev za podršku](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 

7. Unesite svoje podatke za kontakt, a zatim odaberite vrstu kontakta (telefon ili e-pošte). 

8. Odaberite potvrdni okvir **Spremi promjene kontaktima zahtjeva za buduće podrška za** .

9. Kliknite **Stvori**.

Nakon što ste poslali zahtjev, inženjer za podršku će od vas zatražiti da biste nastavili s vaš zahtjev, čim.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Stvaranje sesije podrška u ljusci Windows PowerShell za StorSimple

Da biste riješili probleme koje se mogu pojaviti s uređajem StorSimple, morat ćete sudjelovati s timom Microsoft Support. Microsoft Support možda morati koristiti sesije podrška za prijavu na uređaju. 

Poduzmite sljedeće korake da biste započeli sesiju podrške:

#### <a name="to-start-a-support-session"></a>Pokretanje sesije za podršku

1. Pristup uređaj pomoću serijskog konzole ili putem telnet sesije s udaljenog računala. Da biste to učinili, slijedite korake u [PuTTY koristi za povezivanje s konzole serijski uređaj](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Sesije koji će se otvoriti, pritisnite tipku **Enter** da biste dobili naredbeni redak.

3. Na izborniku serijski konzole odaberite mogućnost 1, **prijavite se pomoću puni pristup**.

4. U naredbeni redak upišite sljedeću lozinku: 

    `Password1`

5. U naredbeni redak upišite sljedeću naredbu:

    `Enable-HcsSupportAccess`

6. Šifrirani niz prikazat će se na vas. Kopirajte niz u uređivaču teksta kao što je blok za pisanje.

7. Spremite ovog niza i pošaljite poruku e-pošte za Microsoft Support. 

> [AZURE.IMPORTANT] Podrška za pristup možete onemogućiti tako da pokrenete `Disable-HcsSupportAccess`. Uređaj StorSimple također će pokušati da biste onemogućili podršku pristup osam sati nakon sesiju pokrenuo. Je najbolji način da biste promijenili vjerodajnice StorSimple uređaj nakon pokretanja sesije podrška.
