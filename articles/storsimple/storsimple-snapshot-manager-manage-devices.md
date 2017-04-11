<properties 
   pageTitle="Upravljanje uređajima s StorSimple snimku Upravitelj | Microsoft Azure"
   description="U članku se opisuje kako pomoću dodatka za BLOG StorSimple snimke Upravitelj da bi povezao i upravljanje uređajima StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>Korištenje upravitelja snimka StorSimple da bi povezao i upravljanje uređajima StorSimple

## <a name="overview"></a>Pregled

Čvorovi u oknu StorSimple snimke Upravitelj **opseg** možete koristiti da biste provjerili uvezenih podataka StorSimple uređaja i Osvježi uređaja povezanih prostora za pohranu. Uz to, kada kliknete čvora **uređaji** , vidjet ćete popis povezanih uređaja i odgovarajuće informacije o stanju u oknu **rezultata** .

![Povezani uređaji](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Slika 1: Povezani StorSimple snimke Upravitelj uređaja** 

Ovisno o odabiri za **Prikaz** u oknu **rezultata** prikazuje sljedeće informacije o svakom uređaju. (Dodatne informacije o konfiguriranju prikaza, otvorite izbornik [Prikaz](storsimple-use-snapshot-manager.md#view-menu).


| Rezultati stupca  |Opis          |
|:----------------|:--------------------| 
| Ime            | Naziv uređaja konfigurirani na portalu za Azure klasični|
| Model           | Broj modela uređaja|
| Verzija         | Verzija softvera instaliranog na uređaj |
| Status          | Je li uređaj dostupna |
| Zadnji put sinkronizirana     | Datum i vrijeme kada je uređaj zadnje sinkronizacije |
| Serijski broj      | Serijski broj za uređaj |
 
Ako desnom tipkom miša kliknite čvor **uređaji** u oknu **opseg** , možete odabrati sljedeće radnje:

- Dodajte ili zamijenite uređaja 
- Priključite uređaj pa provjerite je li uvozi 
- Osvježavanje povezanih uređaja 

Ako kliknite čvor **uređaji** , a zatim desnom tipkom miša kliknite naziv uređaja u oknu s **rezultatima** , možete odabrati sljedeće radnje:

- Autentičnost uređaja 
- Prikaz detalja o uređaju 
- Osvježi uređaja 
- Brisanje Konfiguracija uređaja 
- Promjena lozinke za uređaj

>[AZURE.NOTE] Sve ove akcije dostupne su i u oknu **Akcije** .
 
Pomoću ovog praktičnog vodiča objašnjava kako pomoću upravitelja snimka StorSimple da bi povezao i upravljanje uređajima i izvedite sljedeće zadatke:

- Dodajte ili zamijenite uređaja 
- Priključite uređaj pa provjerite je li uvozi 
- Osvježavanje povezanih uređaja 
- Autentičnost uređaja 
- Prikaz detalja o uređaju 
- Osvježavanje pojedinačni uređaj 
- Brisanje Konfiguracija uređaja 
- Promjena lozinke do istekli uređaja
- Zamjena nije uspjelo uređaja

>[AZURE.NOTE] Općenite informacije o korištenju upravitelja snimka StorSimple sučelja otvorite [Upravitelj snimka StorSimple korisničkog sučelja](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Dodajte ili zamijenite uređaja

Koristite sljedeći postupak da biste dodali ili zamjena StorSimple uređaja.

#### <a name="to-add-or-replace-a-device"></a>Da biste dodali ili zamjena uređaja

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

2. U oknu **opseg** desnom tipkom miša kliknite čvor **uređaji** , a zatim **Konfiguracija uređaja**. Pojavit će se dijaloški okvir **Konfiguracija uređaja** .

    ![Konfiguriranje StorSimple uređaja](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 

3. U padajućem okviru **uređaja** odaberite IP adresa uređaja ili virtualnog uređaja. 

4. U tekstni okvir **Lozinka** unesite lozinku StorSimple snimku Manager koju ste stvorili za uređaj na portalu za Azure klasični. Kliknite **u redu**. Upravitelj snimka StorSimple traži uređaj koji ste naveli. 

    - Ako je uređaj nije dostupan, Upravitelj snimka StorSimple dodaje veze. 

    - Ako uređaj nije dostupan zbog bilo kojeg razloga, Upravitelj snimka StorSimple vraća poruku o pogrešci. Kliknite **u redu** da biste zatvorili poruku o pogrešci, a zatim kliknite **Odustani** da biste zatvorili dijaloški okvir **Konfiguracija uređaja** .

## <a name="connect-a-device-and-verify-imports"></a>Priključite uređaj pa provjerite je li uvozi

Pomoću sljedećeg postupka priključite uređaj StorSimple pa provjerite je li postojeće grupe jedinica koje ste povezali sigurnosne kopije uvezu.

#### <a name="to-connect-a-device-and-verify-imports"></a>Priključite uređaj i provjere uvoza

1. Da biste povezali StorSimple snimke upravitelju uređaja, slijedite upute u članku Dodavanje ili zamjena uređaj. Kada se povezuje s uređajem, Upravitelj snimka StorSimple se odgovori na sljedeći način:

    - Ako uređaj nije dostupan zbog bilo kojeg razloga, Upravitelj snimka StorSimple vraća poruku o pogrešci. 

   - Ako je uređaj nije dostupan, Upravitelj snimka StorSimple dodaje veze. Kada odaberite uređaj, pojavljuje se u oknu s **rezultatima** i polje status označava da je **dostupno**. Upravitelj snimka StorSimple uvozi sve grupe jedinica konfigurirana za uređaj, pod uvjetom da grupe jedinica imate pridružene sigurnosne kopije. Sigurnosne kopije pravila se ne uvoze. Jedinica grupe koje imaju pridružene sigurnosnih kopija se ne uvoze.

2. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

3. Desnom tipkom miša kliknite čvor najviše razine u oknu **opseg** , a zatim kliknite **Uključi/Isključi uvozi prikaz**.

    ![Odaberite Uključi/Isključi uvozi prikaz](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 

4. Pojavit će se dijaloški okvir **Uključi/Isključi uvozi prikaz** prikazuje status uvezene glasnoću grupe i sigurnosno kopiranje. Kliknite **u redu**. 

Nakon što su uspješno uvezeni glasnoću grupe i sigurnosne kopije, StorSimple snimku Manager možete koristiti da biste upravljali njima, baš kao i želite upravljati glasnoću grupe i sigurnosne kopije koju ste stvorili i konfigurirali s StorSimple snimku Upravitelj. 

## <a name="refresh-connected-devices"></a>Osvježavanje povezanih uređaja

Koristite sljedeći postupak da biste sinkronizirali s StorSimple snimku Upravitelja povezanih StorSimple uređaja.

####<a name="to-refresh-connected-devices"></a>Osvježavanje povezanih uređaja

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

2. U oknu **opseg** desnom tipkom miša kliknite **uređaja**, a zatim **Osvježi uređaja**. To sinkronizira povezanih uređaja s StorSimple snimke Upravitelj tako da mogu pregledavati glasnoću grupe i sigurnosne kopije, uključujući sve nedavne dodatke. 

    ![Osvježavanje StorSimple uređaja](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)
 
**Osvježavanje uređaje** akciju dohvaća sve nove grupe jedinica i sve povezane sigurnosne kopije iz povezanih uređaja. Za razliku od **Ponovni pregled količine** akciju za čvor **količine** **Osvježi uređaja** Vraćanje sigurnosne kopije registra.

## <a name="authenticate-a-device"></a>Autentičnost uređaja

Pomoću sljedećeg postupka za provjeru autentičnosti StorSimple uređaj s StorSimple snimke Manager.

#### <a name="to-authenticate-a-device"></a>Za provjeru autentičnosti putem uređaja

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

2. U oknu **opseg** kliknite **uređaji**.

3. U oknu s **rezultatima** desnom tipkom miša kliknite naziv uređaja, a zatim **provjere autentičnosti**.

4. Pojavit će se dijaloški okvir **provjere autentičnosti** . Unesite lozinku za uređaju, a zatim kliknite **u redu**.

    ![Dijaloški okvir za provjeru autentičnosti](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 
 
## <a name="view-device-details"></a>Prikaz detalja o uređaju

Pomoću sljedećeg postupka prikaz detalja o uređaju StorSimple i ako je potrebno, ponovno pokrenite sinkroniziranje s StorSimple snimke Upravitelj uređaja.

#### <a name="to-view-and-resynchronize-device-details"></a>Da biste prikazali i ponovno pokrenite sinkroniziranje Detalji o uređaju

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini.

2. U oknu **opseg** kliknite **uređaji**.

3. U oknu s **rezultatima** desnom tipkom miša kliknite naziv uređaja, a zatim kliknite **Detalji**. 

Pojavit će se dijaloški okvir 4. **Detalji o uređaju** . Ovaj okvir prikazuje naziv, model, verzija, serijski broj, stanje, cilj iSCSI kvalificirani naziv (IQN), a sinkronizacije datum i vrijeme. 

   - Kliknite da biste sinkronizirali uređaj **ponovnog sinkroniziranja** .

   - Kliknite **u redu** ili **Odustani** da biste zatvorili dijaloški okvir.

    ![Detalji o uređaju](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 
 
## <a name="refresh-an-individual-device"></a>Osvježavanje pojedinačni uređaj

Da biste ponovno sinkroniziranje pojedinačni StorSimple uređaj s StorSimple snimke Upravitelj pomoću sljedećeg postupka.

#### <a name="to-refresh-a-device"></a>Da biste osvježili uređaja

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini. 

2. U oknu **opseg** kliknite **uređaji**. 

3. U oknu s **rezultatima** desnom tipkom miša kliknite naziv uređaja, a zatim **Osvježi uređaja**. To s StorSimple snimke Upravitelj sinkronizira uređaj. 

## <a name="delete-a-device-configuration"></a>Brisanje Konfiguracija uređaja

Koristite sljedeći postupak da biste izbrisali iz StorSimple snimke upravitelja pojedinačne konfiguracija StorSimple uređaja.

#### <a name="to-delete-a-device-configuration"></a>Da biste izbrisali Konfiguracija uređaja

1. Kliknite ikonu da biste pokrenuli StorSimple snimke Upravitelj na radnoj površini. 

2. U oknu **opseg** kliknite **uređaji**. 

3. U oknu s **rezultatima** desnom tipkom miša kliknite naziv uređaja, a zatim kliknite **Izbriši**. 

4. Pojavit će se sljedeća poruka. Kliknite **da** da biste izbrisali konfiguraciju ili kliknite **ne** da biste otkazali brisanje.

    ![Brisanje Konfiguracija uređaja](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Promjena lozinke do istekli uređaja

Unesite lozinku za provjeru autentičnosti StorSimple uređaj s StorSimple snimke Upravitelj. Konfiguriranje ovu lozinku prilikom korištenja sučelja komponente Windows PowerShell da biste postavili uređaj. Međutim, lozinku možete isteći. Ako se to dogodi, poslužite se Azure klasični portal za promjenu lozinke. Zatim jer je uređaj konfiguriran u StorSimple snimke Manager prije lozinka istekla, morate ponovno autentičnost u StorSimple snimke Upravitelj uređaja. 

#### <a name="to-change-the-expired-password"></a>Da biste promijenili istekla lozinka

1. Azure klasični portalu pokrenite upravitelj StorSimple servisa.

2. Kliknite **uređaji** > **Konfiguriraj** za uređaj.

3. Pomaknite se prema dolje do odjeljka Upravitelj StorSimple snimke. Unesite lozinku koju je 14 15 znakova. Provjerite je li lozinka sadrži je kombinacije velika slova, mala slova, numeričke i posebnih znakova.

4. Ponovno unesite lozinku da biste je potvrdili.

5. Kliknite **Spremi** pri dnu stranice.

#### <a name="to-re-authenticate-the-device"></a>Da biste ponovno autentičnost uređaja

1. Pokrenite Upravitelj StorSimple snimke.

2. U oknu **opseg** kliknite **uređaji**. Pojavit će se popis konfiguriran u oknu **rezultata** . 

3. Odaberite uređaj, desnom tipkom miša, a zatim kliknite **provjeru autentičnosti**.

4. U prozoru za **provjeru autentičnosti** , unesite novu lozinku. 

5. Odaberite uređaj, desnom tipkom miša i odaberite **Osvježi uređaja**. To s StorSimple snimke Upravitelj sinkronizira uređaj. 

## <a name="replace-a-failed-device"></a>Zamjena nije uspjelo uređaja

Ako StorSimple uređaja ne uspije, a zamjenjuje uređaj čekanja (prebacivanje), poduzmite sljedeće korake za povezivanje na novi uređaj i prikaz pridružene sigurnosne kopije.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Da biste se povezali na novi uređaj nakon prebacivanje

1. Konfigurirajte iSCSI veza na novi uređaj. Upute potražite u odjeljku "korak 7: postavljanja, inicijalizaciju i oblikovanje jedinica" u [uvođenja StorSimple uređaju lokalnog](storsimple-deployment-walkthrough-u2.md). 

>[AZURE.NOTE] Ako na novi uređaj StorSimple ima istu IP adresu kao staru, možda nećete moći povezati stare konfiguracije. 

2. Zaustavljanje servisa za upravljanje Microsoft StorSimple:

    1. Pokrenite Upravitelj poslužitelja.

    2. Na nadzornoj ploči Upravitelj poslužitelja, na izborniku **Alati** odaberite **usluge**. 

    3. U prozoru **usluge** odaberite **Microsoftov servis za upravljanje StorSimple**. 

    4. U desnom oknu u odjeljku **Servis za upravljanje StorSimple Microsoft**, kliknite **Zaustavljanje servisa**. 

3. Uklanjanje konfiguracije podataka koji se odnose na stari uređaj: 

    1. U Eksploreru za datoteke pronađite C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    2. Brisanje datoteka u mapi BACatalog. 

4. Ponovno pokretanje servisa za upravljanje Microsoft StorSimple: 

    1. Na nadzornoj ploči Upravitelj poslužitelja, na izborniku **Alati** odaberite **usluge**. 

    2. U prozoru **usluge** odaberite **Microsoftov servis za upravljanje StorSimple**. 

    3. U desnom oknu u odjeljku **Servis za upravljanje StorSimple Microsoft**, kliknite **ponovno pokrenite servis**. 

5. Pokrenite Upravitelj StorSimple snimke. 

6. Da biste konfigurirali na novi uređaj StorSimple, dovršite korake u korak 2: priključite uređaj StorSimple u [Upravitelju snimku stanja za StorSimple implementacije](storsimple-snapshot-manager-deployment.md). 

7. Desnom tipkom miša kliknite čvor najviše razine u oknu **opseg** (Upravitelj StorSimple snimku u primjeru), a zatim kliknite **Uključi/Isključi uvozi prikaz**. 

8. Kada se vide u StorSimple snimku Manager uvezene glasnoću grupe i sigurnosne kopije, pojavljuje se poruka. Kliknite **u redu**. 

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [pomoću upravitelja snimka StorSimple za administriranje StorSimple rješenje](storsimple-snapshot-manager-admin.md).
- Saznajte kako [pomoću upravitelja snimka StorSimple za prikaz i upravljanje jedinicama](storsimple-snapshot-manager-manage-volumes.md).

