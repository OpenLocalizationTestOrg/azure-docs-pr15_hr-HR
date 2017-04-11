<properties 
   pageTitle="Promjena načina uređaj StorSimple | Microsoft Azure"
   description="Opisuju načini uređaj StorSimple i objašnjava kako pomoću komponente Windows PowerShell za StorSimple da biste promijenili način rada uređaja."
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
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-device-mode-on-your-storsimple-device"></a>Promjena načina uređaj na uređaju StorSimple

Ovaj članak sadrži kratak opis različitih načina rada u kojem možete raditi StorSimple uređaj. Vaš uređaj StorSimple funkcionirati u tri načina: normalno, održavanje i oporavak. 

Kad pročitate članak, znat ćete:

- Su s StorSimple načina uređaja
- Kako da biste utvrdili koji način uređaj StorSimple se
- Kako promijeniti s normalnom održavanja i *obrnuto*


Iznad zadatke upravljanja može izvršiti samo putem sučelja komponente Windows PowerShell StorSimple uređaja.

## <a name="about-storsimple-device-modes"></a>O načinima StorSimple uređaja

Uređaju StorSimple možete raditi u načinu rada normalno, održavanje ili oporavak. Svaki od ovih načina rada ukratko opisane ispod.

### <a name="normal-mode"></a>Normalni način rada

Definira se kao obični operacijski način rada za potpuno konfiguriran StorSimple uređaj. Prema zadanim postavkama, uređaj mora biti u normalnom načinu rada.

### <a name="maintenance-mode"></a>Održavanje

Ponekad uređaj StorSimple možda morati može smjestiti u načinu za održavanje. Ovaj način omogućuje vam održavanje uređaja i njihovu instalaciju disruptive ažuriranja, kao što su oni odnose na disku firmver.

U sustavu možete staviti u načinu za održavanje samo putem komponente Windows PowerShell za StorSimple. Sve zahtjeve za/i pauzirana u tom načinu rada. Servise kao što je koji nije uklonjiv radnom memorijom (NVRAM) ili klasteriranja servisa i zaustaviti. Oba kontrolera ponovno pokrenete kad unesete ili izlazak iz njega u tom načinu rada. Kada ste izašli iz načina rada za održavanje, sve servise će nastaviti i mora biti dobar. To može potrajati nekoliko minuta.

>[AZURE.NOTE]Održavanje **podržano je samo na uređaju ispravno funkcionira. Nije podržana na uređaju u kojem jedan ili oba od kontrolera ne funkcioniraju.**
</br>

### <a name="recovery-mode"></a>Način rada za oporavak

Način rada za oporavak možete opisan "Sigurnom načinu rada sustava Windows s podrškom za mreže". Način rada za oporavak engages tima za Microsoft Support i omogućuje provođenje dijagnostiku u sustavu. Primarnog cilja način rada za oporavak je Dohvaćanje zapisnika sustava.

Ako vaš sustav prelazi u načinu rada za oporavak, potrebno je obratiti Microsoft Support za sljedeće korake. Dodatne informacije potražite [kontakt Microsoftovoj](storsimple-contact-microsoft-support.md)podršci.

>[AZURE.NOTE] **Uređaj ne može smjestiti u načinu rada za oporavak. Ako je uređaj u odgovarajućem stanju, da biste dobili željeni uređaj u stanje u kojem Microsoft Support osoblje možete provjeriti je pozive pokušava način rada za oporavak.**

## <a name="determine-storsimple-device-mode"></a>Odrediti način StorSimple uređaja

#### <a name="to-determine-the-current-device-mode"></a>Da biste odredili način rada za trenutni uređaj

1. Prijavite se na konzolu za serijski uređaj tako da slijedite korake u [PuTTY koristi za povezivanje s konzole serijski uređaj](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Pogledajte natpis poruka na izborniku serijskih konzole uređaja. Ova poruka izričito označava je li uređaj u načinu za održavanje ili oporavak. Ako poruke sadrže određene podatke vezani uz u načinu sustava, uređaj je u običnom načinu rada.

## <a name="change-the-storsimple-device-mode"></a>Promijenite način StorSimple uređaja 

Uređaj StorSimple možete smjestiti u održavanja (iz Normalni način rada) održavanje ili ga instalirati održavanja način ažuriranja. Izvođenje sljedećih postupaka da biste unijeli ili izlazak iz načina rada za održavanje.

> [AZURE.IMPORTANT] Prije nego što unesete održavanja, provjerite jesu li oba uređaja kontrolera dobar tako da pristupite **Hardver Status** na stranice za **Održavanje** Azure klasični portalu. Ako je jedan ili oba kontrolera nisu dobar, obratite se Microsoft Support za sljedeće korake. Dodatne informacije potražite u članku [kontakt Microsoftovoj](storsimple-contact-microsoft-support.md)podršci.

#### <a name="to-enter-maintenance-mode"></a>Da biste unijeli održavanja

1. Prijavite se na konzolu za serijski uređaj tako da slijedite korake u [PuTTY koristi za povezivanje s konzole serijski uređaj](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Na izborniku serijski konzole odaberite mogućnost 1, **prijavite se pomoću puni pristup**. Kada se to od vas zatraži, unesite **uređaju administratorsku lozinku**. Lozinka zadani je: `Password1`.

3. U naredbeni redak upišite 

    `Enter-HcsMaintenanceMode`

4. Prikazat će se poruka upozorenja koja vas obavještava da održavanja će poremetiti sve zahtjeve za/i i sever veza s portala za Azure klasični i zatražit će se za potvrdu. Upišite **Y** da biste unijeli održavanja.

5. Oba kontrolera će se pokrenuti. Po dovršetku ponovno pokretanje drugi će se poruka koja javlja da je u načinu za održavanje uređaja.


#### <a name="to-exit-maintenance-mode"></a>Da biste izašli iz načina rada za održavanje

1. Prijavite se na konzolu serijski uređaj. Provjerite je li u poruku natpis uređaju je u načinu za održavanje.

2. U naredbeni redak upišite:

    `Exit-HcsMaintenanceMode`

3. Pojavit će se poruka upozorenja i poruke o potvrdi. Upišite **Y** da biste izašli iz načina rada za održavanje.

4. Oba kontrolera će se pokrenuti. Po dovršetku ponovno pokretanje drugi će se poruka koja javlja da je uređaj u običnom načinu rada.


## <a name="next-steps"></a>Daljnji koraci

Saznajte kako na uređaju StorSimple [primijeniti normalno i održavanje način ažuriranja](storsimple-update-device.md) .

