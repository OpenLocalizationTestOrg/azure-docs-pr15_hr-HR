<properties 
   pageTitle="Upravljanje zapisima za kontrolu pristupa u StorSimple | Microsoft Azure"
   description="U članku se opisuje kako koristiti kontrole za pristup zapisima (ACRs) da biste utvrdili koji domaćini možete se povezati s jedinice na uređaju StorSimple."
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>Korištenje upravitelja StorSimple servis za upravljanje zapisima za kontrolu pristupa

## <a name="overview"></a>Pregled

Pristup zapisima kontrola (ACRs) omogućuju da biste odredili koji domaćini možete se povezati s glasnoće na uređaju StorSimple. ACRs postavljene na određene glasnoću i sadrže iSCSI kvalificirani imena (IQNs) hosts. Kad na glavno računalo pokuša povezati jedinicu, uređaj provjerava na ACR pridružen toj jedinici za naziv IQN i ako postoji rezultat, zatim uspostavljanja veze. Odjeljak zapisa kontrola pristupa na stranici **Konfiguracija** prikazuje sve zapise kontrole programa access s odgovarajuće IQNs od hosts.

Pomoću ovog praktičnog vodiča objašnjava sljedeće najčešće ACR povezane zadatke:

- Dodavanje zapisa kontrole programa access 
- Uređivanje zapisa kontrole programa access 
- Brisanje zapisa kontrole programa access 

> [AZURE.IMPORTANT] 
> 
> - Kada dodjeljujete programa ACR jedinicu, pobrinuti da jedinica ne istovremeno pristupa više glavnih računala koje nisu grupirani jer to nije oštećena glasnoću. 
> - Prilikom brisanja programa ACR iz jedinice, provjerite je li da odgovarajuće glavno računalo ne pristupa glasnoću Budući da brisanje može rezultirati prekidu za čitanje i pisanje.

## <a name="add-an-access-control-record"></a>Dodajte zapis za kontrole programa access

Da biste dodali ACRs koristite stranice za **Konfiguriranje** StorSimple Upravitelj servisa. Obično ćete povezati jedan ACR s jedna jedinica.

Izvršite sljedeće korake da biste dodali programa ACR.

#### <a name="to-add-an-access-control-record"></a>Da biste dodali zapis za kontrole programa access

1. Na stranici odredišna servisa odaberite uslugu, dvokliknite naziv usluge i pa kliknite karticu **Konfiguracija** .

2. Na popisu tablični u odjeljku **zapisi za kontrolu pristupa**dati **naziv** za svoje ACR.

3. Davanje naziva IQN Windows host u odjeljku **iSCSI pokretač naziv**. Da biste dobili IQN sustava Windows Server glavno računalo, učinite sljedeće:

   - Pokretanje iSCSI pokretaču Microsoft na Windows host.
   - U prozoru **iSCSI pokretač svojstva** na kartici **konfiguracije** odaberite i kopirajte niz iz polja **Naziv pokretač** .
   - Zalijepite niz **iSCSI pokretač naziv** polja u tablici ACRs na portalu za Azure klasični.

4. Kliknite **Spremi** da biste spremili novostvorenu ACR. Tablični popis će se ažurirati ovaj zbrajanja.

## <a name="edit-an-access-control-record"></a>Uređivanje zapisa kontrole programa access

Koristite stranicu **Konfiguriraj** na portalu za Azure klasični da biste uredili ACRs. 

> [AZURE.NOTE] Možete promijeniti samo ACRs koji se trenutno ne koriste. Da biste uredili programa ACR pridružene jedinicu koja se trenutno koristi, morate najprije preuzeti glasnoću izvanmrežno.

Izvršite sljedeće korake da biste uredili programa ACR.

#### <a name="to-edit-an-access-control-record"></a>Da biste uredili zapis za kontrole programa access

1. Na stranici odredišna servisa odaberite uslugu, dvokliknite naziv usluge i pa kliknite karticu **Konfiguracija** .

2. Tablični popis zapisa za kontrolu pristupa, postavite pokazivač iznad ACR koje želite izmijeniti.

3. Navedite novi naziv i/ili IQN za na ACR.

4. Kliknite **Spremi** da biste spremili izmijenjenu ACR. Tablični popis će se ažurirati odrazila ta promjena.

## <a name="delete-an-access-control-record"></a>Brisanje zapisa kontrole programa access

Koristite stranicu **Konfiguriraj** na portalu za Azure klasični da biste izbrisali ACRs. 

> [AZURE.NOTE] Možete izbrisati samo ACRs koji se trenutno ne koriste. Da biste izbrisali programa ACR pridružene jedinicu koja se trenutno koristi, morate najprije preuzeti glasnoću izvanmrežno.

Izvršite sljedeće korake da biste izbrisali zapis za kontrolu pristupa.

#### <a name="to-delete-an-access-control-record"></a>Da biste izbrisali zapis za kontrolu pristupa

1. Na stranici odredišna servisa odaberite uslugu, dvokliknite naziv usluge i pa kliknite karticu **Konfiguracija** .

2. Tablični popis zapisa za kontrolu pristupa (ACRs), postavite pokazivač iznad ACR koji želite izbrisati.

3. Ikona Izbriši (**x**) prikazat će se u ekstremne desnom stupcu za ACR koji ste odabrali. Kliknite ikonu **x** da biste izbrisali na ACR.

4. Kada se zatraži potvrda, kliknite **da** da biste nastavili s brisanjem. Tablični popis će se ažurirati brisanja.

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o [upravljanju StorSimple količine](storsimple-manage-volumes.md).

- Dodatne informacije o [korištenju StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).
 
