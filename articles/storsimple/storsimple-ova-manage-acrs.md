<properties 
   pageTitle="Upravljanje zapisima za kontrolu pristupa za polja virtualne StorSimple | Microsoft Azure"
   description="U članku se opisuje Upravljanje zapisima za kontrolu pristupa (ACRs) da biste utvrdili koji hosts možete se povezati s glasnoću na polja virtualne StorSimple."
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
   ms.date="05/03/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records-for-the-storsimple-virtual-array"></a>Upravljanje zapisima za kontrolu pristupa za polja virtualne StorSimple pomoću upravitelja StorSimple servisa 

## <a name="overview"></a>Pregled

Pristup zapisima kontrola (ACRs) omogućuju da biste odredili koji domaćini možete se povezati s glasnoću na polja virtualne StorSimple (poznat i kao StorSimple lokalnog virtualne uređaj). ACRs postavljene na određene glasnoću i sadrže iSCSI kvalificirani imena (IQNs) hosts. Kada glavno računalo pokušava se povezati s jedinicu, uređaj provjerava ACR pridruženu toj jedinici za naziv IQN, a ako postoji rezultat, zatim uspostavljanja veze. U odjeljku **zapisi kontrola pristupa** na stranici **Konfiguracija** prikazuje sve zapise kontrole programa access s odgovarajuće IQNs od hosts.

Pomoću ovog praktičnog vodiča objašnjava sljedeće najčešće ACR povezane zadatke:

- Početak u IQN
- Dodajte zapis za kontrole programa access 
- Uređivanje zapisa kontrole programa access 
- Brisanje zapisa kontrole programa access 

> [AZURE.IMPORTANT] 
> 
> - Kada dodjeljujete programa ACR jedinicu, pobrinuti da jedinica ne istovremeno pristupa više glavnih računala koje nisu grupirani jer to nije oštećena glasnoću. 
> - Prilikom brisanja programa ACR iz jedinice, provjerite je li da odgovarajuće glavno računalo ne pristupa glasnoću Budući da brisanje može rezultirati prekidu za čitanje i pisanje.

## <a name="get-the-iqn"></a>Početak u IQN

Izvršite sljedeće korake da biste dobili IQN Windows glavno računalo sa sustavom Windows Server 2012.

[AZURE.INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Dodavanje programa ACR

Da biste dodali ACRs koristite stranici za **konfiguraciju** servisa StorSimple Manager. Obično ćete povezati jedan ACR s jedna jedinica.

Informacije o povezivanju programa ACR s jedinice potražite u članku [Dodavanje jedinicu](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).

>[AZURE.IMPORTANT] 
> 
>Kada dodjeljujete programa ACR jedinicu, pobrinuti da jedinica ne istovremeno pristupa više glavnih računala koje nisu grupirani jer to nije oštećena glasnoću.
 
Izvršite sljedeće korake da biste dodali programa ACR.

#### <a name="to-add-an-acr"></a>Da biste dodali programa ACR

1. Na stranici odredišna servisa odaberite uslugu, dvokliknite naziv servisa pa kliknite karticu **Konfiguracija** .

    ![karticu konfiguracija](./media/storsimple-ova-manage-acrs/acr1.png)

2. Na popisu tablični u odjeljku **zapisi za kontrolu pristupa**dati **naziv** za svoje ACR.

3. U odjeljku **iSCSI pokretač naziv**Navedite naziv IQN Windows host. 

4. Kliknite **Spremi** pri dnu stranice da biste spremili novostvorenu ACR. Prikazat će se sljedeća poruka o potvrdi.

    ![poruke o potvrdi](./media/storsimple-ova-manage-acrs/acr2.png)

5. Kliknite ikonu provjeri ![Provjera ikona](./media/storsimple-ova-manage-acrs/check-icon.png). Tablični popis će se ažurirati ovaj zbrajanja.

## <a name="edit-an-acr"></a>Uređivanje u ACR

Koristite stranicu **konfiguracije** na portalu za Azure klasični da biste uredili ACRs. 

> [AZURE.NOTE] Izmijenite samo ACRs koji se trenutno ne koriste. Da biste uredili programa ACR pridružene jedinicu koja se trenutno koristi, morate najprije poduzeti glasnoću izvanmrežno.

Izvršite sljedeće korake da biste uredili programa ACR.

#### <a name="to-edit-an-acr"></a>Da biste uredili programa ACR

1. Na stranici odredišna servisa odaberite uslugu, dvokliknite naziv servisa pa kliknite karticu **Konfiguracija** .

2. Tablični popis zapisa kontrole programa access, postavite pokazivač iznad ACR koje želite izmijeniti.

3. Navedite novi naziv i/ili IQN za na ACR.

4. Kliknite **Spremi** pri dnu stranice da biste spremili izmijenjenu ACR. Prikazat će se poruka potvrde. 

5. Kliknite ikonu provjeri ![Provjera ikona](./media/storsimple-ova-manage-acrs/check-icon.png). Tablični popis će se ažurirati odrazila ta promjena.

## <a name="delete-an-access-control-record"></a>Brisanje zapisa kontrole programa access

Koristite stranicu **konfiguracije** na portalu za Azure klasični da biste izbrisali ACRs. 

> [AZURE.NOTE] 
> 
> - Samo ACRs koji se trenutno ne koriste je potrebno izbrisati. Da biste izbrisali programa ACR pridružene jedinicu koja se trenutno koristi, morate najprije poduzeti glasnoću izvanmrežno.
> - Prilikom brisanja programa ACR iz jedinice, provjerite je li da odgovarajuće glavno računalo ne pristupa glasnoću Budući da brisanje može rezultirati čitanja i pisanja prekidu.

Izvršite sljedeće korake da biste izbrisali zapis za kontrolu pristupa.

#### <a name="to-delete-an-access-control-record"></a>Da biste izbrisali zapis za kontrolu pristupa

1. Na stranici odredišna servisa odaberite uslugu, dvokliknite naziv servisa pa kliknite karticu **Konfiguracija** .

2. Tablični popis zapisa za kontrole programa access (ACRs), postavite pokazivač iznad ACR koji želite izbrisati.

3. Ikona Izbriši (**x**) prikazat će se u ekstremne desnom stupcu za ACR koji ste odabrali. Kliknite ikonu **x** da biste izbrisali na ACR. Prikazat će se sljedeća poruka o potvrdi.

    ![poruke o potvrdi](./media/storsimple-ova-manage-acrs/acr3.png)

5. Kliknite ikonu provjeri ![Provjera ikona](./media/storsimple-ova-manage-acrs/check-icon.png). Tablični popis će se ažurirati brisanja.

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o [dodavanju količine i konfiguriranju ACRs](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).
