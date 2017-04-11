<properties 
   pageTitle="Promjena lozinke na StorSimple | Microsoft Azure" 
   description="U članku se opisuje kako pomoću upravitelja StorSimple servisa da biste promijenili StorSimple snimke upravitelj i uređaja administratorske lozinke." 
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
   ms.workload="TBD" 
   ms.date="08/17/2016"
   ms.author="alkohli"/>

# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>Korištenje upravitelja StorSimple servisa da biste promijenili svoje StorSimple lozinke

## <a name="overview"></a>Pregled 

Azure klasični portal **Konfiguriraj** stranica sadrži sve parametre uređaja možete konfigurirajte na uređaju StorSimple kojim upravlja StorSimple Upravitelj servisa. Pomoću ovog praktičnog vodiča objašnjava kako možete koristiti stranice **Konfiguriraj** da biste promijenili administratora uređaja ili upravitelj snimka StorSimple lozinku.

## <a name="change-the-device-administrator-password"></a>Promjena lozinke administratora uređaja

Prilikom korištenja sučelja komponente Windows PowerShell za pristup StorSimple uređaj, se zatraži da unesete administratorsku lozinku za uređaj. Kada se prvi StorSimple uređaj registrira sa servisom, zadani lozinku za ovo sučelje je *Password1*. Sigurnost vaših podataka, ćete morati promijeniti tu lozinku na kraju postupka registracije. Nije moguće izađete iz postupka registracije bez promjene tu lozinku. Dodatne informacije potražite u članku [korak 3: konfigurirati i registrirati uređaja putem komponente Windows PowerShell za StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Lozinka koju je najprije postaviti putem sučelja komponente Windows PowerShell tijekom registracije mogu se promijeniti putem portala za Azure klasični. Izvršite sljedeće korake da biste promijenili lozinku administratora uređaja.

#### <a name="to-change-the-device-administrator-password"></a>Da biste promijenili lozinku administratora uređaja

1. Na portalu klasični kliknite **uređaji** > **Konfiguriraj** za svoj uređaj.

2. Pomaknite se prema dolje do odjeljka **Lozinku administratora uređaja** . Navedite administratorsku lozinku koja sadrži od 8 15 znakova. Lozinka mora biti kombinacija 3 ili više velika slova, mala slova, numeričke i posebnih znakova.

3. Potvrdite lozinku.

4. Kliknite **Spremi** pri dnu stranice.

Sada treba ažurirati administratorsku lozinku za uređaj. Koristite ovu izmijenjene lozinku za pristup sučelja komponente Windows PowerShell.

## <a name="change-the-storsimple-snapshot-manager-password"></a>Promjena lozinke za Upravitelj StorSimple snimke

Softver StorSimple snimke Upravitelj nalazi na Windows host i administratorima omogućuje upravljanje sigurnosne kopije uređaju StorSimple u obliku lokalno i brze snimke u oblaku.

Prilikom konfiguriranja u StorSimple snimke Upravitelj uređaja, zatražit će se uređaj IP adresa i lozinka za provjeru autentičnosti uređaj za pohranu. Ovu lozinku najprije konfigurirati putem sučelja komponente Windows PowerShell. Dodatne informacije potražite u članku [korak 3: Konfiguriranje i registrirajte uređaj putem komponente Windows PowerShell za StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Lozinka koju je najprije postaviti putem sučelja komponente Windows PowerShell tijekom registracije mogu se promijeniti putem klasične portal. Izvršite sljedeće korake da biste promijenili lozinku Upravitelj StorSimple snimke.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>Da biste promijenili lozinku Upravitelj StorSimple snimke

1. Na portalu klasični kliknite **uređaji** > **Konfiguriraj** za svoj uređaj.

2. Pomaknite se prema dolje do odjeljka **Upravitelj StorSimple snimke** . Unesite lozinku koju je 14 ili 15 znakova. Provjerite je li lozinka sadrži je kombinacija 3 ili više velika slova, mala slova, numeričke i posebnih znakova.

3. Potvrdite lozinku.

4. Kliknite **Spremi** pri dnu stranice.

Lozinka StorSimple snimke Upravitelj treba sada ažurirati.
 

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o [sigurnosti StorSimple](storsimple-security.md).

- Dodatne informacije o [izmjeni konfiguraciji vašeg uređaja](storsimple-modify-device-config.md).

- Dodatne informacije o [korištenju StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).
