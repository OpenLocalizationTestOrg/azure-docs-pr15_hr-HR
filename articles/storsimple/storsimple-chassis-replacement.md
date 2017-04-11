<properties 
   pageTitle="Zamjena nosačima na uređaju StorSimple | Microsoft Azure"
   description="U članku se opisuje kako ukloniti i zamijeniti nosačima za primarni s prilozima StorSimple ili EBOD s prilozima."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-the-chassis-on-your-storsimple-device"></a>Zamjena nosačima na uređaju StorSimple

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča objašnjava uklanjanje i zamjena nosačima na uređaju StorSimple 8000 niz. Model StorSimple 8100 je uređaj s jednom prilozima (jedan nosačima), dok je u 8600 uređaj s dva prilozima (dva nosačima). Za modelu 8600 postoje potencijalno dva nosačima može biti neuspješan uređaj: nosačima za primarni s prilozima ili nosačima za s prilozima EBOD.

U svakom slučaju, nosačima zamjenski otpremljene Microsoft je prazno. Nema Power i hlađenje moduli (PCMs), moduli kontroler pune stanje diskovnih pogona (SSDs), pogona tvrdom disku (HDDs) ili module EBOD bit će uključeni.

>[AZURE.IMPORTANT] Prije uklanjanja i zamjene u nosačima, pregledajte sigurnost informacije u [StorSimple hardver komponente zamjena](storsimple-hardware-component-replacement.md).

## <a name="remove-the-chassis"></a>Uklanjanje na nosačima

Izvršite sljedeće korake da biste uklonili nosačima na uređaju StorSimple.

#### <a name="to-remove-a-chassis"></a>Da biste uklonili nosačima

1. Provjerite je li uređaj StorSimple isključiti i nije povezan s sve izvore power.

2. Uklanjanje mreže i kabeli SAS, ako je primjenjivo.

3. Uklanjanje jedinice za bicikle.

4. Uklanjanje svih pogone i zabilježite slobodnih iz kojeg se uklanjaju. Dodatne informacije potražite u članku [Uklanjanje pogon diska](storsimple-disk-drive-replacement.md#remove-the-disk-drive).

5. Na na EBOD s prilozima (Ako je to je nosačima koja se nije uspjela), uklonite kontroler modula EBOD. Dodatne informacije potražite u članku [Uklanjanje programa EBOD kontroler](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller). 

    Na u primarni s prilozima (Ako je to je nosačima koja se nije uspjela), uklonite na kontrolera i Imajte na umu slobodnih iz kojeg se uklanjaju. Dodatne informacije potražite u članku [Uklanjanje kontroler](storsimple-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Instalacija sustava nosačima

Izvršite sljedeće korake da biste instalirali na nosačima na uređaju StorSimple.

#### <a name="to-install-a-chassis"></a>Da biste instalirali nosačima

1. Postavljanje nosačima u za bicikle. Dodatne informacije potražite u članku [za bicikle postavljanja uređaju StorSimple 8100](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) ili [za bicikle postavljanja StorSimple 8600 uređaj](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).

2. Nakon što u nosačima je postavljen na bicikle, instalirajte moduli kontroler istim položajima koji su već instalirani u.

3. Instalirati pogonima na istoj položaje i slobodnih koji su već instalirani u.

    >[AZURE.NOTE] Preporučujemo da prvo instalirajte na SSDs na slobodnih, a zatim instalirajte na HDDs.

2. Uređaj postavljen za bicikle i instalirane komponente, povezivanje uređaja s izvorima odgovarajuću power i uključite uređaj. Detalje potražite u članku [StorSimple 8100 uređaja](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) ili [kabel StorSimple 8600 uređaj](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [StorSimple hardvera komponente zamjenu](storsimple-hardware-component-replacement.md).

