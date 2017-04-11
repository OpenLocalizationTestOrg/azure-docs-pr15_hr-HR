<properties 
   pageTitle="Deaktiviranje i brisanje uređaja StorSimple | Microsoft Azure"
   description="U članku se opisuje uklanjanje uređaja StorSimple iz servisa tako da najprije deaktiviranje i brisanje ga."
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
   ms.date="10/18/2016"
   ms.author="anoobbacker" />

# <a name="deactivate-and-delete-a-storsimple-device"></a>Deaktiviranje i brisanje StorSimple uređaja

## <a name="overview"></a>Pregled

Možda želite poduzeti StorSimple uređaj iz servisa (na primjer, ako su zamjene ili nadogradnje uređaju ili ako više ne koristite StorSimple). Ako je to slučaj, morat ćete deaktivirati uređaj prije no što ga izbrišete. Deaktivacija severs veze između uređaja i odgovarajuće StorSimple Upravitelj servisa. Pomoću ovog praktičnog vodiča objašnjava uklanjanje StorSimple uređaj iz servisa po prvi deaktivacije ga, a zatim je izbrišete. 

Kada deaktivirate uređaj, sve podatke koji su spremljeni lokalno na uređaj neće biti dostupne. Može obnoviti samo podaci koji su povezani s uređajem koji je pohranjen u oblaku.  

>[AZURE.WARNING] Deaktivacija je trajno operacija i nije moguće poništiti. Neaktivne uređaj se ne može registrirati sa servisom StorSimple Upravitelj osim u slučaju da najprije vratiti tako da na tvorničke. 
>
>Na tvorničke ponovno postaviti postupak briše sve podatke koji su spremljeni lokalno na uređaj. Stoga je ključna Izrada snimke oblaka sve podatke prije deaktivirate uređaj. To će vam omogućiti da biste oporavili sve podatke u noviji fazi.

Pomoću ovog praktičnog vodiča u članku se objašnjava kako:

- Deaktivirajte uređaja i brisanje podataka
- Deaktiviranje uređaja i zadržali podatke

Također objašnjava kako Deaktivacija i brisanje funkcionira na uređaju virtualne StorSimple.

>[AZURE.NOTE] Prije nego što deaktivirate StorSimple fizički ili virtualni uređaj, provjerite je li da biste zaustavili ili izbrisali klijenti i hosts koje ovise o tom uređaju.

## <a name="deactivate-and-delete-data"></a>Deaktiviranje i brisanje podataka

Ako vas zanimaju potpuno brisanje uređaja, a ne želite zadržati podatke na uređaju, provedite sljedeće korake.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Da biste deaktivirali uređaj i brisanje podataka  

1. Prije no što deaktivacije uređaj, morate izbrisati sve glasnoće spremnika (i jedinice) povezan s uređajem. Količinsko spremnika možete izbrisati tek nakon što ste izbrisali pridružene sigurnosne kopije.

2. Deaktiviranje uređaj na sljedeći način:

    1. Na stranici upravitelja StorSimple servisa **uređaje** , odaberite uređaj koji želite deaktivirati pa pri dnu stranice kliknite **Deaktiviraj**.

    2. Pojavit će se poruka potvrde. Kliknite **da** da biste nastavili. Postupak Deaktiviraj pokrenut će se i potrajati nekoliko minuta.

3. Nakon deaktivacije, možete potpuno brisanje uređaja. Brisanje uređaja je uklanja s popisa uređaja povezanih s uslugom. Servis možete zatim više ne možete upravljati izbrisane uređaja. Da biste izbrisali uređaj, slijedite ove korake:

    1. Na stranici upravitelja StorSimple servisa **uređaja** , odaberite deaktiviran uređaj koji želite izbrisati.

    2. Na dnu stranice kliknite **Izbriši**.

    3. Zatražit će se za potvrdu. Kliknite **da** da biste nastavili.

    Može potrajati nekoliko minuta za uređaj će se izbrisati.

## <a name="deactivate-and-retain-data"></a>Deaktiviranje i zadržali podatke

Ako su zainteresirani za brisanje uređaja, ali želite zadržati podatke, provedite sljedeće korake.

####<a name="to-deactivate-a-device-and-retain-the-data"></a>Da biste deaktivirali uređaja i zadržali podatke 

1. Deaktiviranje uređaj. Ostat će sve spremnike glasnoću i brze snimke uređaja.

    1. Na stranici upravitelja StorSimple servisa **uređaje** , odaberite uređaj koji želite deaktivirati pa pri dnu stranice kliknite **Deaktiviraj**.

    2. Pojavit će se poruka potvrde. Kliknite **da** da biste nastavili. Postupak Deaktiviraj pokrenut će se i potrajati nekoliko minuta.

2. Sada možete uspjeti nad spremnika glasnoću i pridruženi snimke. Postupak, idite na [Prebacivanje i Izrada oporavak za svoj uređaj StorSimple](storsimple-device-failover-disaster-recovery.md).

3. Nakon deaktivacije i prebacivanje, možete potpuno brisanje uređaja. Brisanje uređaja je uklanja s popisa uređaja povezanih s uslugom. Servis možete zatim više ne možete upravljati izbrisane uređaja. Izvršite sljedeće korake da biste izbrisali uređaj:
 
    1. Na stranici upravitelja StorSimple servisa **uređaja** , odaberite deaktiviran uređaj koji želite izbrisati.

    2. Na dnu stranice kliknite **Izbriši**.

    3. Zatražit će se za potvrdu. Kliknite **da** da biste nastavili.

    Može potrajati nekoliko minuta za uređaj će se izbrisati.

## <a name="deactivate-and-delete-a-virtual-device"></a>Deaktiviranje i brisanje virtualnog uređaja

Za uređaj virtualne StorSimple, deaktivacija deallocates virtualnog računala. Nakon toga možete izbrisati virtualnog računala i resursima stvara kada ga je dodijeljena. Kada je deaktiviran virtualnog uređaja, nije moguće vratiti u prethodno stanje. 

Deaktivacija rezultate u sljedeće radnje:

- Uklanja se StorSimple virtualnog uređaja.

- Uklanjaju se OSDisk i diskova podataka stvorene za StorSimple virtualnog uređaja.

- Zadržavaju se hostira servisa i virtualne mreže koje su stvorene tijekom dodjele resursa. Ako se ne koriste te entiteti, trebali biste ih izbrisati ručno.

- Zadržavaju se snimke oblaka stvorio StorSimple virtualnog uređaja.

## <a name="next-steps"></a>Daljnji koraci
- Da biste vratili deaktiviran uređaja na tvorničke zadane vrijednosti, idite na [izvorne postavke uređaja na tvorničke postavke](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

- Za tehničku pomoć, [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md).

- Da biste saznali više o tome kako koristiti servis StorSimple Manager, otvorite [Upravitelj StorSimple servis za administriranje StorSimple uređaj](storsimple-manager-service-administration.md). 
