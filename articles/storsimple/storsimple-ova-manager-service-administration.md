<properties 
   pageTitle="Upravitelj StorSimple virtualne polja Administracija | Microsoft Azure"
   description="Informirajte se o upravljanju virtualne polja lokalnog StorSimple pomoću upravitelja StorSimple servisa Azure klasični portalu."
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
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-virtual-array"></a>Korištenje upravitelja StorSimple servisa za administraciju sustava StorSimple virtualne polja

![tijek postupka za postavljanje](./media/storsimple-ova-manager-service-administration/manage4.png)

## <a name="overview"></a>Pregled

U ovom se članku opisuje sučelje servisa StorSimple Upravitelj kako povezati i različite mogućnosti dostupne, uključujući i navode veze na određene tijekova rada koji se mogu obaviti putem ovog korisničkog Sučelja. 

Kad pročitate članak, znat ćete kako:

- Povezivanje sa servisom za Upravitelj StorSimple
- Otvorite upravitelj UI StorSimple
- Administriranje putem servisa StorSimple Upravitelj vaše StorSimple virtualne polja

> [AZURE.NOTE] Da biste vidjeli mogućnosti upravljanja za uređaj niz StorSimple 8000, idite na članak [Korištenje StorSimple Upravitelj servisa za administraciju uređaju StorSimple](storsimple-manager-service-administration.md).

## <a name="connect-to-the-storsimple-manager-service"></a>Povezivanje sa servisom za Upravitelj StorSimple

Servis za upravitelja StorSimple pokreće se u Microsoft Azure i povezat će više polja virtualne StorSimple. Središnje Microsoft Azure klasični portal izvodi u pregledniku omogućuju upravljanje sljedećim uređajima. Povezivanje sa servisom StorSimple upravitelj, učinite sljedeće:

#### <a name="to-connect-to-the-service"></a>Da biste se povezali sa servisom

1. Idite na [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

2. Koristi vjerodajnice za Microsoftov račun, prijavite se na portal Microsoft Azure klasični (koja se nalazi u gornjem desnom kutu okna).

3. Pomaknite se prema dolje u lijevom navigacijskom oknu da biste pristupili StorSimple Upravitelj servisa.

    ![Pomaknite se do servisa](./media/storsimple-ova-manager-service-administration/admin-scroll.png)

## <a name="navigate-the-storsimple-manager-service-ui"></a>Otvorite upravitelj StorSimple servisa korisničkog Sučelja

Navigacijske hijerarhije StorSimple Upravitelj servisa korisničkog Sučelja prikazuju se u tablici u nastavku.

- Odredišna stranica **Upravitelj StorSimple** vodi vas na odnosi se na sve virtualne polja unutar servisa korisničkog Sučelja razini usluge stranice.

- Stranici **uređaji** vodi vas na razini uređaja – korisničkog Sučelja stranice primjenjuju na određene virtualne polja.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>Upravitelj StorSimple servisa navigacijske hijerarhije

|Odredišna stranica|Stranica o razini usluge|Stranica razini uređaja|
|---|---|---|
|Servis upravitelja StorSimple|Nadzorna ploča (servis)|Nadzorna ploča (uređaj)|
||Uređaji →|Monitora|
||Katalog|Zajedničko korištenje (poslužitelj datoteka) ili </br>Količine (iSCSI server)|
||Konfiguriranje (servis)|Konfiguriranje (uređaj)|
||Zadaci|Održavanje|
||Upozorenja|

## <a name="use-the-storsimple-manager-service-to-perform-management-tasks"></a>Korištenje usluge StorSimple Upravitelj za izvođenje zadataka upravljanja

Sljedeća tablica prikazuje sažetak uobičajene zadatke upravljanja i složenih tijekova rada koji se izvesti u sklopu StorSimple Upravitelj servisa korisničkog Sučelja. Zadaci su organizirane prema na stranicama korisničkog Sučelja na kojem se pokreće.

Dodatne informacije o svaki tijek rada kliknite odgovarajući postupak u tablici.

#### <a name="storsimple-manager-workflows"></a>Upravitelj StorSimple tijekova rada

|Ako želite da biste to učinili...|Prijelaz na ovoj stranici korisničkog Sučelja...|Koristite ovaj postupak|
|---|---|---|
|Stvaranje servisa</br>Brisanje servisa</br>Registracija ključ servisa</br>Obnovi ključa za registraciju servisa|Servis upravitelja StorSimple|[Uvođenje servisa StorSimple Manager](storsimple-ova-manage-service.md)|
|Promjena ključa za šifriranje podataka usluge</br>U zapisnicima operacije|StorSimple Upravitelj servisa → nadzorne ploče|[Korištenje nadzorna ploča službe za StorSimple](storsimple-ova-service-dashboard.md)|
|Deaktiviranje virtualne polja</br>Brisanje virtualne polja|Upravitelj StorSimple servisa → uređaja|[Deaktiviranje i brisanje virtualne polja](storsimple-ova-deactivate-and-delete-device.md)|
|Izrada prebacivanje oporavak i uređaja</br>Prebacivanje preduvjeti</br>Prebacivanje na virtualnog uređaja</br>Oporavak Izrada tvrtke continuity (BCDR)</br>Pogreške tijekom Izrada oporavak|Upravitelj StorSimple servisa → uređaja|[Izrada prebacivanje oporavak i uređaja za vaše StorSimple virtualne polja](storsimple-ova-failover-dr.md)|
|Stvaranje sigurnosne kopije dionice i količine</br>Iskoristite ručno sigurnosno kopiranje</br>Promijenite raspored sigurnosnog kopiranja</br>Prikaz postojeće sigurnosne kopije|Upravitelj StorSimple servisa → sigurnosne kopije kataloga|[Stvaranje sigurnosne kopije vaše StorSimple virtualne polja](storsimple-ova-backup.md)|
|Vraćanje dionice iz skupa sigurnosne kopije</br>Vraćanje količine iz skupa sigurnosne kopije</br>Oporavak na razini stavke (samo za datoteke poslužitelja)|StorSimple Upravitelj servisa → sigurnosne kopije kataloga|[Vraćanje iz sigurnosne kopije vaše StorSimple virtualne polja](storsimple-ova-restore.md)|
|O računima za pohranu</br>Dodavanje računa za pohranu</br>Uređivanje prostora za pohranu računa</br>Brisanje računa za pohranu|Konfiguriranje → StorSimple Upravitelj servisa|[Upravljanje računima za pohranu za polja virtualne StorSimple](storsimple-ova-manage-storage-accounts.md)|
|Zapisi za kontrolu pristupa</br>Dodavanje ili izmjena zapis za kontrolu pristupa </br>Brisanje zapisa kontrole programa access|Konfiguriranje → StorSimple Upravitelj servisa|[Upravljanje zapisima za kontrolu pristupa za polja virtualne StorSimple](storsimple-ova-manage-acrs.md)|
|Prikaz detalja o posla|Upravitelj StorSimple servisa → poslove| [Upravljanje zadacima StorSimple virtualne polja](storsimple-ova-manage-jobs.md)|
|Konfiguriranje postavki upozorenja</br>Primanje upozorenja</br>Upravljanje upozorenjima</br>Pregled upozorenja|Upravitelj StorSimple service → upozorenja|[Prikaz i upravljanje upozorenjima za polja virtualne StorSimple](storsimple-ova-manage-alerts.md)|
|Izmjena administratorsku lozinku za uređaj|Uređaji StorSimple Upravitelj servisa → → konfiguriranje|[Promjena lozinke administratora uređaja StorSimple virtualne polja](storsimple-ova-change-device-admin-password.md)|
|Instalacija ažuriranja|Uređaji StorSimple Upravitelj servisa → → održavanja|[Ažurirajte vaše virtualne polja](storsimple-ova-install-update-01.md)|

>[AZURE.NOTE] [Lokalni web korisničkog Sučelja](storsimple-ova-web-ui-admin.md) potrebno je koristiti za sljedeće zadatke:
>
>- [Dohvaćanje ključa za šifriranje podataka usluge](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
>- [Stvaranje paketa za podršku](storsimple-ova-web-ui-admin.md#generate-a-log-package)
>- [Zaustavite i ponovno pokrenite virtualne polja](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)

##<a name="next-steps"></a>Daljnji koraci
Informacije o web korisničkog Sučelja i kako ga koristiti, otvorite da biste [koristili web StorSimple korisničkog Sučelja za administraciju sustava StorSimple virtualne polja](storsimple-ova-web-ui-admin.md).
