<properties 
   pageTitle="Promjena lozinke za administratore StorSimple virtualnog uređaja | Microsoft Azure"
   description="U članku se opisuje kako pomoću portala za Azure klasični ili StorSimple virtualne polja web korisničkog Sučelja za promjenu lozinke administratora uređaja."
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
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-storsimple-virtual-array-device-administrator-password"></a>Promjena lozinke administratora uređaja StorSimple virtualne polja

## <a name="overview"></a>Pregled

Prilikom korištenja sučelja komponente Windows PowerShell za pristup StorSimple virtualnog uređaja, se zatraži da unesete administratorsku lozinku za uređaj. Kada uređaj StorSimple najprije dodjeli i rada, lozinku zadani je *Password1*. Sigurnost vaših podataka, lozinku zadani ističe prvi put da se prijavite, a zatim se od vas zatraži da biste promijenili tu lozinku.

Možete koristiti i lokalne web korisničkog Sučelja ili portala za Azure klasični da biste promijenili lozinku administratora uređaja u bilo kojem trenutku kada uređaj je implementiran u vašem radnom okruženju. Svaki od tih postupaka je opisan u ovom članku.

## <a name="use-the-azure-classic-portal-to-change-the-password"></a>Pomoću portala za Azure klasični Promjena lozinke

Izvršite sljedeće korake da biste promijenili lozinku administratora uređaja putem portala za Azure klasični.

#### <a name="to-change-the-device-administrator-password-via-the-azure-classic-portal"></a>Da biste promijenili lozinku administratora uređaja putem portala za Azure klasični

1. Na portalu kliknite **uređaji** > **konfiguracije** za svoj uređaj.

2. Pomaknite se prema dolje do odjeljka **Lozinku administratora uređaja** . Navedite administratorsku lozinku koja sadrži od 8 15 znakova. Lozinka mora biti kombinaciju velika slova, mala slova, numeričke i posebnih znakova.

3. Potvrdite lozinku.

4. Kliknite **Spremi** pri dnu stranice.

Sada treba ažurirati administratorsku lozinku za uređaj. Koristite ovu izmijenjene lozinku za pristup lokalno na uređaj.

## <a name="use-the-storsimple-virtual-array-web-ui-to-change-the-password"></a>Korištenje web StorSimple virtualne polja korisničko Sučelje za promjenu lozinke

Izvršite sljedeće korake da biste promijenili administratorsku lozinku uređaja putem lokalnog web korisničkog Sučelja.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Da biste promijenili lozinku administratora uređaja putem lokalnog web korisničkog Sučelja

1. Na lokalnom web-mjestu korisničkog Sučelja, kliknite **Održavanje** > **Promjena lozinke** za svoj uređaj.

    ![Promjena lozinke1](./media/storsimple-ova-change-device-admin-password/image40.png)

2. Upišite **trenutnu lozinku**.

3. Upišite **novu lozinku**. Lozinka mora imati najmanje 8 znakova. Mora sadržavati 3 od 4 od sljedećeg: velika slova, mala slova, numeričke i posebnih znakova.

    Imajte na umu da lozinku ne mogu biti jednaki zadnje 24 lozinke.

3. Ponovno unesite lozinku da biste je potvrdili.

    ![Promjena password2](./media/storsimple-ova-change-device-admin-password/image41.png)

4. Pri dnu stranice, kliknite **Primijeni**. Primijenit će se zatim novu lozinku. Ako promijenite lozinku ne uspije, vidjet ćete sljedeće pogreške.

    ![Pogreška lozinke](./media/storsimple-ova-change-device-admin-password/image42.png)

    Kada uspješno ažurira lozinku, bit ćete obaviješteni. Možete koristiti ovu izmijenjene lozinku da biste pristupili lokalno na uređaj.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [administraciji sustava StorSimple virtualne polja](storsimple-ova-web-ui-admin.md).
