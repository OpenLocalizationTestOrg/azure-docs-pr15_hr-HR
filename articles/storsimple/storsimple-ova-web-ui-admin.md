<properties 
   pageTitle="StorSimple virtualne polja web administracije korisničkog Sučelja | Microsoft Azure"
   description="U članku se opisuje izvođenje osnovni uređaj zadataka administracije putem virtualne polja StorSimple web korisničkog Sučelja."
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
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>Administriranje sustava StorSimple virtualne polja pomoću korisničkog Sučelja Web

![tijek postupka za postavljanje](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Pregled

Vodiči za u ovom članku primjenjuju se na Microsoft Azure StorSimple virtualne polja (poznat i kao StorSimple lokalnog virtualne uređaj) radi izdanje 2016 ožujak Općenito dostupan (GA). U ovom se članku opisuju neke od složenih tijekova rada i upravljanje zadaci koji se mogu izvršiti na polja virtualne StorSimple. Možete upravljati StorSimple virtualne polja pomoću upravitelja StorSimple servisa korisničkog Sučelja (naziva se na portal korisničkog Sučelja) i lokalne web korisničkog Sučelja za uređaj. U ovom se članku usredotočuje se na zadatke koje možete izvršiti pomoću web korisničkog Sučelja.

Ovaj članak sadrži sljedeće upute:

- Dobivanje ključa za šifriranje podataka usluge
- Otklonite pogreške prilikom instalacije korisničkog Sučelja za web
- Generiranje paket zapisnika
- Isključivanje i ponovno pokrenite uređaj

## <a name="get-the-service-data-encryption-key"></a>Dobivanje ključa za šifriranje podataka usluge

Kada registrirate prvi uređaj sa servisom StorSimple Upravitelj generira se ključa za šifriranje podataka servisa. Ovaj ključ pa je obavezna ključa za registraciju servisa da biste registrirali dodatnih uređaja sa servisom StorSimple Manager.

Ako ste izgubili ključ za šifriranje podataka servisa i potrebno je dohvatiti, provedite sljedeće korake na lokalnom web-mjestu korisničkog Sučelja uređaja registrirana na servisu.

#### <a name="to-get-the-service-data-encryption-key"></a>Da biste dobili ključa za šifriranje podataka usluge

1. Povezivanje s lokalnom web korisničkog Sučelja. Idite na **Konfiguracija** > **oblaka postavke**.
  

2. Pri dnu stranice kliknite **Dohvati ključa za šifriranje podataka servisa**. Pojavit će se ključ. Kopiranje i spremite ovaj ključ.
    
    ![ključ servisa podataka šifriranje 1](./media/storsimple-ova-web-ui-admin/image27.png)
   


## <a name="troubleshoot-web-ui-setup-errors"></a>Otklonite pogreške prilikom instalacije korisničkog Sučelja za web

U nekim slučajevima kada konfigurirate uređaja putem lokalnog web korisničko Sučelje, koje možete naići pogreške. Dijagnosticiranje i otklanjanje poteškoća s takve pogreške, možete pokrenuti dijagnostičkih testova.

#### <a name="to-run-the-diagnostic-tests"></a>Da biste pokrenuli dijagnostičkih testova

1. U lokalnoj web korisničkog Sučelja idite na **Otklanjanje poteškoća** > **dijagnostičkih testova**.

    ![pokrenite dijagnostiku 1](./media/storsimple-ova-web-ui-admin/image29.png)

2. Pri dnu stranice kliknite **Pokreni dijagnostičkih testova**. To će pokrenuti testove dijagnosticiranje moguće probleme s mrežom, uređaj, web proxy vremena ili oblak postavke. Bit ćete obaviješteni da na uređaju instaliran testira.

3. Nakon što ste dovršili testova, prikazat će se rezultati. Sljedeći primjer prikazuje rezultate dijagnostičkih testova. Primijetite da su postavke proxyja web nije konfiguriran na ovom uređaju i zbog toga ne pokreće web proxy test. Sve ostale testovi za postavke mreže, DNS poslužitelj i postavke vremena bilo uspješno.

    ![pokrenite dijagnostiku 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Generiranje paket zapisnika

Paket zapisnika sastoji se od svih relevantnih zapisnika koje vam mogu pomoći Microsoft Support s otklanjanje problema s bilo kojeg uređaja. U ovom izdanju zapisnika paket možete generirati putem lokalnog web korisničkog Sučelja.

#### <a name="to-generate-the-log-package"></a>Da biste generirali paketa zapisnika

1. U lokalnoj web korisničkog Sučelja idite na **Otklanjanje poteškoća** > **sustava zapisnika**.

    ![Generiranje zapisnika paket 1](./media/storsimple-ova-web-ui-admin/image31.png)

2. Pri dnu stranice kliknite **Stvori zapisnika paketa**. Paket sustava zapisnike stvorit će se. To može potrajati nekoliko minuta.

    ![Stvaranje paketa zapisnika 2](./media/storsimple-ova-web-ui-admin/image32.png)

    Bit ćete obaviješteni nakon paket uspješno stvorili, a da biste naznačili vrijeme i datum stvaranja paket će se ažurirati na stranicu.

    ![Stvaranje paketa zapisnika 3](./media/storsimple-ova-web-ui-admin/image33.png)

3. Kliknite **preuzmite paket zapisnika**. Zipane paket će se preuzeti na računalu.

    ![Stvaranje paketa zapisnika 4](./media/storsimple-ova-web-ui-admin/image34.png)

4. Možete raspakiraj paketa preuzete zapisnika i pregledavati datoteke zapisnika sustava.

## <a name="shut-down-and-restart-your-device"></a>Isključivanje i ponovno pokrenite uređaj

Možete isključiti ili ponovno pokrenite virtualne uređaj putem lokalnog weba korisničkog Sučelja. Ne možemo preporučeno koje prije no što ponovno pokrenete, poduzeti količine ili zajedničko korištenje izvan mreže na glavnom računalu, a zatim željeni uređaj. To će smanjiti bilo koju mogućnost oštećenja podataka. 

#### <a name="to-shut-down-your-virtual-device"></a>Da biste isključili virtualnog uređaja

1. U lokalnoj web korisničkog Sučelja idite na **Održavanje** > **Power postavke**.

2. Pri dnu stranice kliknite **isključivanje**.

    ![isključivanje uređaja 1](./media/storsimple-ova-web-ui-admin/image36.png)

3. Pojavit će se upozorenje koja govori da zatvaranja uređaja prekinuti sve IO koji su u tijeku, rezultira na nedostupnost. Kliknite ikonu provjeri ![Provjera ikona](./media/storsimple-ova-web-ui-admin/image3.png).

    ![uređaj isključivanje upozorenja](./media/storsimple-ova-web-ui-admin/image37.png)

    Bit ćete obaviješteni da je u isključivanje pokrenuo.

    ![isključivanje uređaj rada](./media/storsimple-ova-web-ui-admin/image38.png)

    Uređaj će sada se isključiti. Ako želite započeti uređaju, morat ćete učiniti putem upravitelja Hyper-V.

#### <a name="to-restart-your-virtual-device"></a>Da biste ponovno pokrenuli virtualnog uređaja

1. U lokalnoj web korisničkog Sučelja idite na **Održavanje** > **Power postavke**.

2. Pri dnu stranice kliknite **ponovno pokrenite**.

    ![ponovno pokretanje uređaja](./media/storsimple-ova-web-ui-admin/image36.png)

3. Pojavit će se upozorenje koja govori da ponovnog pokretanja uređaja će se zaustaviti sve IOs koji su u tijeku, rezultira na nedostupnost. Kliknite ikonu provjeri ![Provjera ikona](./media/storsimple-ova-web-ui-admin/image3.png).

    ![ponovno pokrenite upozorenje](./media/storsimple-ova-web-ui-admin/image37.png)

    Bit ćete obaviješteni da je ponovno pokretanje pokrenuo.

    ![ponovno pokretanje pokrenut](./media/storsimple-ova-web-ui-admin/image39.png)

    Dok je u tijeku je ponovno pokretanje, izgubit ćete vezu korisničkog Sučelja. Ponovno pokretanje možete pratiti tako da povremeno osvježite korisničkog Sučelja. Osim toga, možete nadzirati status ponovnog pokretanja uređaja putem upravitelja Hyper-V.

## <a name="next-steps"></a>Daljnji koraci

Saznajte kako [koristiti StorSimple Upravitelj servisa za upravljanje uređaj](storsimple-manager-service-administration.md).
