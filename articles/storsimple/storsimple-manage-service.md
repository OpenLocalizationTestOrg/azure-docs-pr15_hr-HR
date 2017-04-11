<properties 
   pageTitle="Uvođenje servisa Upravitelj StorSimple | Microsoft Azure"
   description="U članku se objašnjava kako stvoriti i brisanje StorSimple Upravitelj servisa na portalu za Azure klasični, a u članku se opisuje kako upravljati ključa za registraciju servisa."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="deploy-the-storsimple-manager-service"></a>Uvođenje servisa StorSimple Manager

## <a name="overview"></a>Pregled

Servis za upravitelja StorSimple pokreće se u Microsoft Azure i povezuje s više StorSimple uređaja. Kada stvorite servis, možete je koristiti za upravljanje uređajima s portala klasični Microsoft Azure izvodi u pregledniku. Omogućuje praćenje svih uređaja koji su povezani sa servisom StorSimple Upravitelj iz jedne, središnje mjesto, čime ćete minimiziranje teret administratora.

Odredišna stranica Upravitelj StorSimple navodi sve servise StorSimple Manager možete koristiti za upravljanje uređajima StorSimple prostora za pohranu. Za svaki servis StorSimple Upravitelj sljedeći podaci prikazuju se na stranici upravitelja StorSimple:

- **Naziv** – naziv koji je dodijelio usluzi Upravitelj StorSimple kada je stavka stvorena. Naziv servisa nije moguće promijeniti nakon stvaranja servis.

- **Status** – status servisa, što može biti **aktivna**, **Stvaranje**ili **Internetu**.

- **Mjesto** – geografski u kojem će biti implementirano StorSimple uređaja.

- **Pretplata** – naplate pretplate koja je povezana sa servisima.

Uobičajeni zadaci koje se mogu izvršiti na stranici upravitelja StorSimple su:

- Stvaranje servisa
- Brisanje servisa
- Registracija ključ servisa
- Obnovi ključa za registraciju servisa

Pomoću ovog praktičnog vodiča opisuje kako izvesti svaki od tih zadataka.

## <a name="create-a-service"></a>Stvaranje servisa

Mogućnost **Brzo stvaranje** koristite da biste stvorili StorSimple Upravitelj servisa ako želite uvesti uređaju StorSimple. Da biste stvorili usluga, morate koristiti:

- Pretplate s Enterprise Agreement
- Aktivni prostora za pohranu račun za Microsoft Azure
- Podaci o naplati koji se koristi za upravljanje pristupom

Možete odabrati i da biste generirali zadani prostor za pohranu račun prilikom stvaranja servis.

Jedan servis možete upravljati više uređaja. Međutim, uređaj ne može obuhvaćati većem broju servisa. Velike tvrtke može imati više instanci servisa za rad s različitim pretplate, tvrtke ili ustanove ili čak i implementacija mjesta. Napominjemo da želite zasebne pojavljivanja StorSimple Upravitelj servisa za upravljanje StorSimple 8000 niz uređaji i StorSimple virtualne polja.

Izvršite sljedeće korake da biste stvorili servisa.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

## <a name="delete-a-service"></a>Brisanje servisa

Prije brisanja servisa, provjerite je li da nema povezanih uređaja ga koristite. Ako se servis koristi, deaktivirajte povezanih uređaja. Operacija Deaktiviraj će sever veze između uređaja i servis, ali sačuvati uređaj podataka u oblaku. 

[AZURE.IMPORTANT] Nakon brisanja servisa operacija nije moguće poništiti. Bilo kojeg uređaja koji je pomoću servisa bit će potrebno tvorničke prije nego što se može se koristiti s drugih servisa za ponovno postavljanje. U ovom scenariju lokalnih podataka na uređaju, kao i konfiguraciji, izgubit će se.

Izvršite sljedeće korake da biste izbrisali servisa.

### <a name="to-delete-a-service"></a>Da biste izbrisali servisa

1. Na stranici **upravitelja StorSimple servisa** odaberite servis koji želite izbrisati.

1. Kliknite **Izbriši** pri dnu stranice.

1. U obavijesti o potvrdi, kliknite **da** . Može potrajati nekoliko minuta za servis će se izbrisati.

## <a name="get-the-service-registration-key"></a>Registracija ključ servisa

Kada uspješno stvorite servisa, morat ćete registrirati uređaju StorSimple sa servisom. Da biste registrirali prvi uređaj za StorSimple, trebat će vam ključa za registraciju servisa. Da biste registrirali dodatnih uređaja s postojeći servis za StorSimple, trebat će vam ključa za registraciju i ključa za šifriranje podataka servisa (koja nastaje na prvom uređaju tijekom registracije). Dodatne informacije o ključa za šifriranje servisa podataka potražite u članku [StorSimple sigurnost](storsimple-security.md). Ključ Registracija tako da pristupite **Ključa za registraciju** na stranici **servisa** .

Izvršite sljedeće korake da biste dobili ključa za registraciju servisa.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

Zadrži servisa ključa za registraciju na sigurnom mjestu. Trebat će vam taj ključ, kao i usluge podataka ključa za šifriranje, da biste registrirali dodatnih uređaja s taj servis. Nakon dobivanja ključa za registraciju servisa, morate konfigurirati uređaja putem komponente Windows PowerShell radi StorSimple sučelja.

Detalje o korištenju ključa za registraciju potražite u članku [korak 3: konfigurirati i registrirati uređaja putem komponente Windows PowerShell za StorSimple](storsimple-deployment-walkthrough.md#step-2-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Obnovi ključa za registraciju servisa

Morat ćete Obnovi ključa za registraciju servisa ako koje su potrebne za izvođenje ključa zakretanje ili na popisu administratori servisa promijenio. Kada Obnovi ključ, novi ključ koristi se samo za registriranje sljedeće uređaje. Uređaji koji su već registrirane su ostaju isti za taj proces.

Izvršite sljedeće korake da biste Obnovi ključa za registraciju servisa.

### <a name="to-regenerate-the-service-registration-key"></a>Da biste Obnovi ključa za registraciju servisa

1. Na stranici **upravitelja StorSimple servisa** kliknite **Ključa za registraciju**.

1. U dijaloškom okviru **Ključa za registraciju** kliknite **Obnovi**.

1. Prikazat će se poruka potvrde. Pritisnite **u redu** za nastavak s na regeneration.

1. Pojavit će se novi ključ usluga registracije.

1. Kopirajte ovu tipku i spremite ga registrirali novi uređaje na taj servis.

1. Kliknite ikonu provjeri ![Ikonu Provjera](./media/storsimple-manage-service/HCS_CheckIcon.png) Da biste zatvorili ovog dijaloškog okvira.


## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o [obradi StorSimple implementacije](storsimple-deployment-walkthrough.md).

- Dodatne informacije o [upravljanju računa StorSimple prostora za pohranu](storsimple-manage-storage-accounts.md).

- Saznajte više o tome kako [koristite StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).

 
