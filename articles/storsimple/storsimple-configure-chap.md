<properties 
   pageTitle="Konfiguriranje CHAP za svoj uređaj StorSimple | Microsoft Azure"
   description="U članku se opisuje kako konfigurirati test rukovanja provjeru autentičnosti protokolom (CHAP) na uređaju StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-chap-for-your-storsimple-device"></a>Konfiguriranje CHAP za svoj uređaj StorSimple

Pomoću ovog praktičnog vodiča objašnjava kako konfigurirati CHAP za svoj uređaj StorSimple. Postupak detaljne u ovom članku odnosi na StorSimple 8000 niz, kao i StorSimple 1200 uređaja.

CHAP skraćenica za provjeru autentičnosti protokol za ispitivanje rukovanja. To je shema za provjeru autentičnosti poslužitelja koristi provjera identiteta udaljenim klijentima. Provjera temelji se na zajedničkom lozinke ili tajna. CHAP može biti jednosmjerna (unidirectional) ili zajednički (dvosmjerni). Jednosmjerna CHAP je kada cilj potvrđuje u pokretaču. Međusobna ili obrnutim CHAP s druge strane, potreban je cilj autentičnost pokretač, a zatim pokretač autentičnost cilj. Provjera autentičnosti Pokretač može se implementirati bez provjere autentičnosti cilj. Međutim, provjere autentičnosti ciljne se implementirati samo ako pokretač provjeru autentičnosti i implementirana. 

Kao preporučenim načinom rada, preporučujemo da koristite CHAP za poboljšanje sigurnosti iSCSI.

>[AZURE.NOTE] Imajte na umu da IPSEC trenutno nije podržan na StorSimple uređajima.

Postavke CHAP na uređaju StorSimple moguće je konfigurirati na sljedeće načine:

- Provjera autentičnosti unidirectional ili jednosmjerna

- Dvosmjeran ili međusobna ili obrnutim provjere autentičnosti

U svakoj od ovih slučajeva portal za uređaj i poslužiteljski softver za pokretač iSCSI potrebno je konfigurirati. Detaljne upute za tu konfiguraciju opisane su sljedeći praktičnog vodiča.

## <a name="unidirectional-or-one-way-authentication"></a>Provjera autentičnosti unidirectional ili jednosmjerna

U unidirectional provjeru autentičnosti, cilj potvrđuje pokretač. Ova provjera autentičnosti zahtijeva Konfiguriranje postavki pokretač CHAP na uređaju StorSimple i iSCSI pokretač softver na računalu koje hostira. Detaljne postupke za StorSimple uređaju i Windows host opisane su sljedeće.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Da biste konfigurirali uređaja radi jednosmjerna provjere autentičnosti

1. Azure klasični portalu na stranici **uređaja** , kliknite karticu **Konfiguracija** .

    ![Pokretač CHAP](./media/storsimple-configure-chap/IC740943.png)

2. Pomaknite se prema dolje na ovoj stranici, a zatim u odjeljku **CHAP pokretač** :
                                                    
    1. Navedite korisničko ime za vaše pokretač CHAP.

    2. Navedite lozinku za vaš pokretač CHAP.

         > [AZURE.IMPORTANT] CHAP korisničko ime mora sadržavati manje od 233 znakova. Lozinka CHAP mora biti između 12 i 16 znakova. Korisničko ime i lozinka za dulje rezultirat će neuspješne provjere autentičnosti na glavnom računalu za Windows.
    
    3. Potvrdite lozinku.

4. Kliknite **Spremi**. Prikazat će se poruka potvrde. Kliknite **u redu** da biste spremili promjene.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Da biste konfigurirali jednosmjerna provjere autentičnosti na poslužitelju glavnog računala za Windows

1. Na poslužitelju Windows glavno računalo, pokrenite iSCSI pokretač.

2. U prozoru **iSCSI pokretač svojstva** poduzeti sljedeće korake:
                                                    
    1. Kliknite karticu za **predočavanje elektroničkih dokumenata** .

        ![Svojstva pokretač iSCSI](./media/storsimple-configure-chap/IC740944.png)

    2. Kliknite **otkrijte Portal**.

3. U dijaloškom okviru **Otkrivanje Portal cilj** :
                                                    
    1. Navedite IP adresa uređaja.

    3. Kliknite **Napredno**.

        ![Uvod u ciljnu portal](./media/storsimple-configure-chap/IC740945.png)

4. U dijaloškom okviru **Napredne postavke** :
                                                    
    1. Potvrdite okvir **Omogući CHAP prijavite** .

    2. U polje **naziv** navedite korisničko ime koje ste naveli za pokretač CHAP klasični portalu.

    3. U polje **cilj tajna** navedite lozinku koju ste naveli za CHAP pokretaču portalu za klasični.

    4. Kliknite **u redu**.

        ![Napredne postavke Općenito](./media/storsimple-configure-chap/IC740946.png)

5. Na kartici **ciljnih web-mjesta** u prozoru **iSCSI pokretač svojstva** status uređaja prikazivati kao **povezan**. Ako koristite uređaj StorSimple 1200, zatim svakoj će biti postavljena kao odredište iSCSI kao što je prikazano u nastavku. Dakle, korake 3-4 morat ćete se ponavljati za svaku jedinicu.

    ![Količine postavljena kao zasebna ciljnih web-mjesta](./media/storsimple-configure-chap/chap4.png)

    > [AZURE.IMPORTANT] Ako promijenite naziv iSCSI novi naziv će se koristiti za nove sesije iSCSI. Nove postavke ne koriste se za postojeće sesije dok se ne odjavite i prijavite ponovno.

Dodatne informacije o konfiguriranju CHAP u sustavu Windows server glavno računalo, idite na [dodatna pitanja vezana uz](#additional-considerations).


## <a name="bidirectional-or-mutual-authentication"></a>Dvosmjeran ili zajednički provjere autentičnosti

U Dvosmjeran provjeru autentičnosti, cilj potvrđuje pokretač, a zatim pokretač potvrđuje cilj. Potreban je korisnik da biste konfigurirali postavke pokretač CHAP, kao i obrnutim CHAP postavke uređaja i iSCSI pokretač softver na glavnom računalu. Postupci u nastavku opisuju koraci koje morate konfigurirati međusobna provjera autentičnosti na uređaju i na glavnom računalu za Windows.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Da biste konfigurirali uređaja radi međusobna provjera autentičnosti

1. Azure klasični portalu na stranici **uređaja** , kliknite karticu **Konfiguracija** .

    ![Cilj CHAP](./media/storsimple-configure-chap/IC740948.png)

2. Pomaknite se prema dolje na ovoj stranici, a zatim u odjeljku **CHAP cilj** :
                                                    
    1. Navedite **Obrnuti CHAP korisničko ime** za svoj uređaj.

    2. Navedite **Obrnuti CHAP lozinku** za svoj uređaj.

    3. Potvrdite lozinku.

3. U odjeljku **CHAP pokretač** :
                                                
    1. Navedite **korisničko ime** za svoj uređaj.

    1. Upišite **lozinku** za svoj uređaj.

    3. Potvrdite lozinku.

4. Kliknite **Spremi**. Prikazat će se poruka potvrde. Kliknite **u redu** da biste spremili promjene.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Da biste konfigurirali Dvosmjeran provjere autentičnosti na poslužitelju glavnog računala za Windows

1. Na poslužitelju Windows glavno računalo, pokrenite iSCSI pokretač.

2. U prozoru **iSCSI pokretač svojstva** kliknite karticu **Konfiguracija** .

3. Kliknite **CHAP**.

4. U dijaloškom okviru **iSCSI pokretač međusobna tajna CHAP** :
                                                    
    1. Upišite **Obrnuti CHAP lozinke** koje ste konfigurirali Azure klasični portalu.

    2. Kliknite **u redu**.

        ![iSCSI pokretač međusobna CHAP tajna](./media/storsimple-configure-chap/IC740949.png)

5. Kliknite karticu **ciljnih web-mjesta** .

6. Kliknite gumb **za povezivanje** . 

7. U dijaloškom okviru **Povezivanje cilj** kliknite **Dodatno**.

8. U dijaloškom okviru **Dodatna svojstva** :
                                                    
    1. Potvrdite okvir **Omogući CHAP prijavite** .

    2. U polje **naziv** navedite korisničko ime koje ste naveli za pokretač CHAP klasični portalu.

    3. U polje **cilj tajna** navedite lozinku koju ste naveli za CHAP pokretaču portalu za klasični.

    4. Potvrdite okvir **Izvedi međusobna provjere autentičnosti** .

        ![Napredne postavke međusobna provjera autentičnosti](./media/storsimple-configure-chap/IC740950.png)

    5. Kliknite **u redu** da biste dovršili konfiguraciju CHAP
     
Dodatne informacije o konfiguriranju CHAP u sustavu Windows server glavno računalo, idite na [dodatna pitanja vezana uz](#additional-considerations).

## <a name="additional-considerations"></a>Dodatne napomene

Značajka **Brzog povezivanje** ne podržava veza koje sadrže CHAP omogućena. Kada je omogućen CHAP, obavezno koristite gumb **za povezivanje** koji je dostupan na kartici **ciljnih web-mjesta** za povezivanje s cilj.

![Povezivanje s cilj](./media/storsimple-configure-chap/IC740947.png)

U dijaloškom okviru **za povezivanje s ciljnom** , koji se prikazuju odaberite potvrdni okvir **Dodaj ove veze na popis favorita ciljnih web-mjesta** . Time se osigurava da svaki put ponovnog pokretanja računala, pokušaju da biste vratili veze na iSCSI omiljene ciljnih web-mjesta.

## <a name="errors-during-configuration"></a>Pogreške tijekom konfiguracija

Ako vašoj konfiguraciji CHAP nije valjana, pa ćete vidjeti poruku o pogrešci **neuspjele provjere autentičnosti** .

## <a name="verification-of-chap-configuration"></a>Provjera konfiguracije CHAP

Možete provjeriti da CHAP koristi na sljedeći način.

#### <a name="to-verify-your-chap-configuration"></a>Da biste provjerili konfiguraciju CHAP

1. Kliknite **Omiljene ciljnih web-mjesta**.

2. Odaberite ciljnu datoteku za koju ste omogućili provjeru autentičnosti.

3. Kliknite **Detalji**.

    ![iSCSI pokretač svojstva omiljene ciljnih web-mjesta](./media/storsimple-configure-chap/IC740951.png)

4. U dijaloškom okviru **Omiljene cilj pojedinosti** Imajte na umu unos u polje za **provjeru autentičnosti** . Ako konfiguraciju je bio uspješan, trebalo bi pisati **CHAP**.

    ![Detalji o omiljene cilj](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o [sigurnosti StorSimple](storsimple-security.md).

- Dodatne informacije o [korištenju StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).
