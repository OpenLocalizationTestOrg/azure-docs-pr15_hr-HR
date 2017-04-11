<properties 
   pageTitle="Mijenjati podatke 0 postavke na uređaju StorSimple | Microsoft Azure"
   description="Saznajte kako pomoću komponente Windows PowerShell za StorSimple da ponovno konfigurira 0 mrežno sučelje podataka na uređaju StorSimple."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="modify-the-data-0-network-interface-settings-on-your-storsimple-device"></a>Izmjena 0 mrežne postavke sučelje za podatke na uređaju StorSimple

## <a name="overview"></a>Pregled

Vaš uređaj Microsoft Azure StorSimple ima šest sučelje mreže, iz podataka 0 podataka 5. Podaci 0 sučelje uvijek konfiguriran putem sučelja komponente Windows PowerShell ili serijski konzole i automatski oblak omogućeno. Imajte na umu da ne možete konfigurirati sučelje 0 mreže podataka pomoću portala za Azure klasični. 

PODATAKA 0 sučelja najprije je konfiguriran pomoću čarobnjaka za postavljanje tijekom početno uvođenje StorSimple uređaja. Kada je uređaj u radu sa servisom načinu rada, možda ćete morati ponovno konfigurira podataka 0 postavke. Pomoću ovog praktičnog vodiča nudi dva načina za izmjenu podataka 0 mrežne postavke i do komponente Windows PowerShell za StorSimple.

Kad pročitate ovog praktičnog vodiča, će se moći:

- Mijenjati podatke 0 mrežne postavke kroz Čarobnjak za postavljanje
- Izmjena postavke 0 mrežnih podataka putem na `Set-HcsNetInterface` cmdleta



## <a name="modify-data-0-network-settings-through-setup-wizard"></a>Izmjena postavke 0 mrežnih podataka pomoću čarobnjaka za postavljanje
Postavke 0 mrežnih podataka možete konfigurirajte tako da povezujete sučelje komponente Windows PowerShell StorSimple uređaja i pokretanje sesije Čarobnjak za postavljanje. Izvršite sljedeće korake za izmjenu podataka 0 postavke:

#### <a name="to-modify-data-0-network-settings-through-setup-wizard"></a>Da biste izmijenili postavke 0 mrežnih podataka pomoću čarobnjaka za postavljanje

1. Na izborniku serijski konzole odaberite mogućnost 1, **prijavite se pomoću puni pristup**. Kada se to od vas zatraži **uređaj administratorsku lozinku**. Lozinka zadani je `Password1`.

2. U naredbeni redak upišite:

    `Invoke-HcsSetupWizard`

3. Pojavit će se čarobnjak za postavljanje za konfiguriranje podataka 0 sučelja uređaja. Unesite nove vrijednosti za IP adresu, pristupnika i maska mreže.

> [AZURE.NOTE] Fixed kontrolera IP-ovi ćete biti rekonfigurirati na stranici **Konfiguracija** StorSimple uređaja na portalu za Azure klasični. Dodatne informacije, idite na [sučelje mreže Izmijeni](storsimple-modify-device-config.md#modify-network-interfaces).


## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a>Izmjena postavke 0 mrežnih podataka pomoću cmdleta skup HcsNetInterface
Drugi način da ponovno konfigurira podataka 0 mrežno sučelje je putem the use na `Set-HcsNetInterface` cmdlet. Cmdlet se izvršava putem sučelja komponente Windows PowerShell StorSimple uređaja. Kada koristite ovaj postupak, kontrolerom fiksno IP-ovi mogu se konfigurirati ovdje. Izvršite sljedeće korake za izmjenu podataka 0 postavke: 

#### <a name="to-modify-data-0-network-settings-through-the-set-hcsnetinterface-cmdlet"></a>Da biste izmijenili postavke 0 mrežnih podataka pomoću cmdleta skup HcsNetInterface

1. Na izborniku serijski konzole odaberite mogućnost 1, **prijavite se pomoću puni pristup**. Kada se to od vas zatraži lozinku administratora uređaja. Lozinka zadani je `Password1`.

2. U naredbeni redak upišite:

    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
    
    U uglatim zagradama kutni podataka 0 unesite sljedeće vrijednosti:
                                            
    - IPv4 adresa
    
    - IPv4 pristupnika
    
    - IPv4 masku.
    
    - FIXED IPv4 adresa za kontroler 0

    - FIXED IPv4 adresa za 1 kontroler

    Dodatne informacije o korištenju ovaj cmdlet potražite u članku [Windows](https://technet.microsoft.com/library/dn688161.aspx)PowerShell za StorSimple cmdlet referencu.

## <a name="next-steps"></a>Daljnji koraci

- Da biste konfigurirali sučelje mreže osim podataka 0, možete koristiti [Konfiguriraj stranice na portalu za Azure klasični](storsimple-modify-device-config.md). 

- Ako se pojave problemi prilikom konfiguriranja sučelja za mrežu, pročitajte [Otklanjanje poteškoća za implementaciju](storsimple-troubleshoot-deployment.md).

